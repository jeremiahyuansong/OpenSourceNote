# MyTinySTL
 - 这是一个由C++实现的常用stl容器的开源项目，项目地址在[MyTinySTL](https://github.com/Alinshans/MyTinySTL)，写这个笔记的主要目的是为了巩固自己的C++知识以及常用的stl的操作。
 
## 项目目录
MyTinySTL的文件夹下包含下面这些文件，可以看到常见的vector、list、map等容器类。还有一些不知道的文件，暂且不管。希望后面能够一一介绍到。  

.
├── algo.h
├── algobase.h
├── algorithm.h
├── alloc.h
├── allocator.h
├── astring.h
├── basic_string.h
├── construct.h
├── deque.h
├── exceptdef.h
├── functional.h
├── hashtable.h
├── heap_algo.h
├── iterator.h
├── list.h
├── map.h
├── memory.h
├── numeric.h
├── queue.h
├── rb_tree.h
├── set.h
├── set_algo.h
├── stack.h
├── type_traits.h
├── uninitialized.h
├── unordered_map.h
├── unordered_set.h
├── util.h
└── vector.h

## 类介绍

### allocator && construct
allocator类定义在文件allocator.h中,主要功能包括：定义常用的typename、分配内存空间、构造对象、析构对象、释放内存空间。  

- _定义常用的typename_

```C++
template <class T>
class allocator
{
public:
  typedef T            value_type;
  typedef T*           pointer;
  typedef const T*     const_pointer;
  typedef T&           reference;
  typedef const T&     const_reference;
  typedef size_t       size_type;
  typedef ptrdiff_t    difference_type;
  // ...
}
```

在后面的vector、list等stl的实现中可以看到，对类型的引用都是在allocator的基础上重新定义 
 
```C++
  // vector 的嵌套型别定义
  typedef mystl::allocator<T>                      allocator_type;
  typedef mystl::allocator<T>                      data_allocator;

  typedef typename allocator_type::value_type      value_type;
  typedef typename allocator_type::pointer         pointer;
  typedef typename allocator_type::const_pointer   const_pointer;
  typedef typename allocator_type::reference       reference;
  typedef typename allocator_type::const_reference const_reference;
  typedef typename allocator_type::size_type       size_type;
  typedef typename allocator_type::difference_type difference_type;
```

对于这种使用，我的理解：除了后面要依赖allocator的内存分配和构造，还有一个用处是便于后面更改所有容器的类型定义，怎么理解这句话呢？举个栗子：现在allocator中将*指针*为T\*, typedef为pointer, *引用*为T&，typedef为reference。这样就指定了后面vecotr、list的指针和引用也都是这种类型。但是我们假设有一天三体人最终入侵并占领了地球，他们规定，以后引用特指T*, 指针特指T&，那这时候我们只用更改allocator中的typedef即可，这样的更改同样作用到后面自己实现的vector和list等容器。反之假设我们没有allocator中的定义，那我们将会在vector和list中分别定义引用和指针各自的形式，当三体人提出了这样的无理需求的时候，我们就只能蒙蔽的去所有容器实现中一一进行更改。

-  _分配内存空间_  
allocator中将对象的内存空间分配和对象构造两个步骤分开， allocate可以分配单个对象和多个对象需要的内存空间。
	- *note* 这个技术依赖于C++提供的新版操作符，不同于原来的new T同时分配内存和调用构造函数，新的方式用::operator new分配内存空间， Placement new方式来在已分配空间的内存上进行对象的构造。具体详情见[C++官方](https://en.cppreference.com/w/cpp/language/new)中的Allocation和Placement new。

```C++
  // 分配空间
  static T*   allocate();
  static T*   allocate(size_type n);
  
  template <class T>
T* allocator<T>::allocate()
{
  return static_cast<T*>(::operator new(sizeof(T)));
}

  template <class T>
T* allocator<T>::allocate(size_type n)
{
  if (n == 0)
    return nullptr;
  return static_cast<T*>(::operator new(n * sizeof(T)));
}
```

- _构造对象_  
to be continue...
