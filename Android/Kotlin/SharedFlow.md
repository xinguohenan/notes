SharedFlow里有一个Array, 每个collect()调用都会网Array里添加一个element:
```
internal abstract class AbstractSharedFlow<S : AbstractSharedFlowSlot<*>> : SynchronizedObject() {
    @Suppress("UNCHECKED_CAST")
    protected var slots: Array<S?>? = null // allocated when needed
...

    override suspend fun collect(collector: FlowCollector<T>) {
        // Add a new slot into slots array.
        val slot = allocateSlot()
        ...
    }
    
    override suspend fun emit(value: T) {
        // Call tryEmit()
        if (tryEmit(value)) return // fast-path
        emitSuspend(value)
    }
    
    override fun tryEmit(value: T): Boolean {
        var resumes: Array<Continuation<Unit>?> = EMPTY_RESUMES
        val emitted = synchronized(this) {
            if (tryEmitLocked(value)) {
                // Call findSlotsToResumeLocked
                resumes = findSlotsToResumeLocked(resumes)
                true
            } else {
                false
            }
        }
        for (cont in resumes) cont?.resume(Unit)
        return emitted
    }    
    
    private fun findSlotsToResumeLocked(resumesIn: Array<Continuation<Unit>?>): Array<Continuation<Unit>?> {
        var resumes: Array<Continuation<Unit>?> = resumesIn
        var resumeCount = resumesIn.size
        // Call forEachSlotLocked
        forEachSlotLocked loop@{ slot ->
            val cont = slot.cont ?: return@loop // only waiting slots
            if (tryPeekLocked(slot) < 0) return@loop // only slots that can peek a value
            if (resumeCount >= resumes.size) resumes = resumes.copyOf(maxOf(2, 2 * resumes.size))
            resumes[resumeCount++] = cont
            slot.cont = null // not waiting anymore
        }
        return resumes
    }

    // Call collect() method for each collector.
    protected inline fun forEachSlotLocked(block: (S) -> Unit) {
        if (nCollectors == 0) return
        slots?.forEach { slot ->
            if (slot != null) block(slot)
        }
    }
}



```
