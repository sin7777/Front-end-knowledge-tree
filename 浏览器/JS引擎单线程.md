# JS单线程

## setTimeout和setInterval

也就是说setTimeout只能保证在指定的时间过后将任务(需要执行的函数)插入队列等候，并不保证这个任务在什么时候执行。执行javascript的线程会在空闲的时候，自行从队列中取出任务然后执行它。javascript通过这种队列机制，给我们制造一个异步执行的假象。
