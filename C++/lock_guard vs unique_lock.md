https://stackoverflow.com/a/20516924

lock_guard and unique_lock are pretty much the same thing; lock_guard is a restricted version with a limited interface.

A lock_guard always holds a lock from its construction to its destruction. A unique_lock can be created without immediately locking, can unlock at any point in its existence, and can transfer ownership of the lock from one instance to another.

So you always use lock_guard, unless you need the capabilities of unique_lock. A condition_variable needs a unique_lock.

