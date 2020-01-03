# Java_concurrency_in_practice

______________

1. 理论
   - Since the actions of a thread accessing a stateless object cannot affect the correctness of operations in other threads, stateless objects are threadͲsafe.
     - 由于线程访问无状态对象`没有字段，也没有引用其他类的字段`的动作不会影响其他线程中操作的正确性，因此无状态对象是线程安全的。
   - A race condition occurs when the correctness of a computation depends on the relative timing or interleaving of multiple threads by the runtime;
     - 当计算的正确性取决于运行时多个线程的相对定时或交织时，就会发生竞争状态。
   - The most common type of race condition is check-then-act, where a stale observation is used to make a decision on what to do next.
     - 竞赛条件最常见的类型是“先检查后行动”，其中使用过时的观察结果来决定下一步该做什么。