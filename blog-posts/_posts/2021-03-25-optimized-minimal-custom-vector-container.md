---
layout: post
title: Optimized Minimal Custom Vector Container
description: >
  A minimal approach to create optimized Custom Vector Container in C++, covering 'Rule of Five' & implementing the concept of 'Mapping Objects on Dynamically Allocated Buffer'.
hide_image: false
accent_color: '#4fb1ba'
accent_image:
  background: 'linear-gradient(to bottom,#193747 0%,#233e4c 30%,#3c929e 50%,#d5d5d4 70%,#cdccc8 100%)'
  overlay:    true
---

Hey Lads, so I've been working on Custom approach for improving & optimizing the **STL::Vector** class as **contiguous container**. In this regard, I also recently shared two posts on our [**Facebook Group**](https://www.facebook.com/groups/450885176097764) for improving Vector class by implementing [**Rule of Three**](/assets/blog_material/Rule_of_Three.pdf) (covering copy-semantics) & further [**Rule of Five**](https://www.internalpointers.com/post/c-rvalue-references-and-move-semantics-beginners)  (covering move-semantics).

Now, after implementing both of these Rules, I realized that still my Vector class was inefficient to carry out larger operations. So, I did my further research & found out that these Vectors basically work by separate **Allocation** & **Construction**. Yes, you heard it right. **Allocation** & **Construction** are two separate concepts which you normally implement both at same time by declaring **object** or **array of objects** with **'new/malloc'** keyword.

So, **STL::Vector** initially only allocates & **reserves** the space for your objects & holds the construction part (i.e. you do this by using reserve **modifier**), and when user decides to use this space (i.e. by using push_back, emplace_back or resize modifier), the Vector constructs the objects on that **dynamically** allocated space (called **Buffer** on **Heap**).

Now, here comes the role of [**placement new**](https://www.geeksforgeeks.org/placement-new-operator-cpp/#:~:text=Placement%20new%20is%20a%20variation,object%20in%20the%20passed%20memory.) operator which basically takes the **address of buffer** & constructs the object of given structure or class on that position of buffer (i.e. we usually take **array of char** type to create buffer).

After allocation & construction of these objects, comes the **accessing** part. Well, It was kind of tricky for me but I managed to find out (thanks to stackoverflow) that the objects created by **placement new** operator maintains like **void** type status. Means you can access them by **type-casting** the **buffer-pointer** to the type of **objects** & voila, you are ready to use the optimized Vector class.

**P.S:** For better understanding, I have provided a lot of **test-cases** seperately in below attached **Source Code Folder** (which are also well-commented) & they almost cover all of the use-cases of my custom Vector class. Also, I have used an [**Instance-Counter**](https://tristanbrindle.com/posts/beware-copies-initializer-list) class in some test-cases to keep the track of Constructions & Destructions of objects in Vector class. If there is anything ambiguous, feel free to reach out. Peace!!!

[**Source Code Folder Link**](https://github.com/HypertextAssassin0273/Data_Structures_in_Cpp/tree/main/Native_Data_Structures/Contiguous_Structures)
