# circle_object_buf
#enum class Status : std::uint8_t：定义了一个枚举类型Status，包含三个值：OK表示操作成功，TIMEOUT表示超时，EMPTY表示缓冲区为空。

CircleObjectBuffer(const size_t &max_size)：构造函数，创建一个CircleObjectBuffer对象。max_size参数指定了缓冲区的最大容量。

bool is_empty()：检查缓冲区是否为空，使用互斥锁保护访问。

size_t size()：返回当前缓冲区中元素的个数，使用互斥锁保护访问。

Status push(const T &element)：向缓冲区中压入一个元素。使用互斥锁保护访问，将元素添加到缓冲区末尾，并在必要时删除最早的元素。之后通过条件变量通知等待的线程。

Status pop(T &element)：从缓冲区中弹出一个元素，并将其存储在element参数中。如果缓冲区为空，调用线程将被阻塞，直到有元素可用。

Status pop_with_timeout(T &element, std::int32_t timeout)：从缓冲区中弹出一个元素，并将其存储在element参数中。如果缓冲区为空，调用线程将被阻塞，直到有元素可用，或者超过指定的超时时间。

Status pop_without_block(T &element)：从缓冲区中弹出一个元素，并将其存储在element参数中。如果缓冲区为空，立即返回EMPTY状态。

bool is_empty_without_lock()：无锁版本的is_empty()函数，检查缓冲区是否为空。

size_t size_without_lock()：无锁版本的size()函数，返回当前缓冲区中元素的个数。

Status push_without_lock(const T &element)：无锁版本的push()函数，向缓冲区中压入一个元素。

Status pop_without_lock(T &element)：无锁版本的pop()函数，从缓冲区中弹出一个元素，并将其存储在element参数中
