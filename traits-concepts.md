Traits and concepts
===================

In C++, traits, templates, concepts and constraints are all used to achieve similar goals: to constrain the types of
arguments that can be passed to a function or to constrain the types of template arguments.

- Traits are a way to check certain properties of a type at compile time. Traits are typically implemented using
  template classes or structs, and they usually inherit from `std::true_type` or `std::false_type` depending on whether
  the type has the property in question or not. They can be used in conjunction with template
  metaprogramming, `enable_if` and SFINAE to constrain the types of arguments that can be passed to a function or to
  constrain the types of template arguments.

- Templates are a way to create generic functions and classes that can work with any type. They are typically used to
  create generic algorithms that can work with different types of data, and they can be specialized for specific types
  to achieve better performance. Templates can be used in combination with traits and SFINAE to constrain the types of
  arguments that can be passed to a function or to constrain the types of template arguments.

- Concepts are a C++20 feature that provide a way to formally specify the requirements of a template parameter. They
  provide a way to describe the set of types that are valid arguments for a function or template and they can be used to
  improve code readability and error messages. Concepts are used with `requires` keyword to specify the set of types
  that are valid arguments for a function or template.

- Constraints are a way to specify the requirements of a function or template using the `requires` keyword. They can be
  used to specify the conditions that must be met by the arguments passed to a function or template. Constraints can be
  used in combination with concepts, traits and SFINAE to constrain the types of arguments that can be passed to a
  function or to constrain the types of template arguments.

In general, constraints, concepts and traits are all ways to specify the requirements of a function or template and to
constrain the types of arguments that can be passed to them. They can be used together to achieve more powerful and
flexible type checking.

For example, you can use a trait to check if a type has a certain property, such as being a container or having a
specific member function. You can then use a concept to specify that a template parameter must be of a certain type,
such as a number or a container. Finally, you can use constraints to specify more complex requirements, such as the size
of a container or the value of a number.

By using these features together, you can create more powerful and flexible templates that can be used with a wide range
of types, while still ensuring that only types that meet the specified requirements can be used.

It's also worth noting that C++20 introduced the `std::type_identity` type trait, which allows to extract the type of an
expression, and `std::same_as` which allows to check if a type is the same as another type. This can be used in
combination with concepts and constraints to further constrain the types of arguments and template parameters.

In summary, traits, templates, concepts, and constraints are all powerful features in C++ that can be used together to
achieve more powerful and flexible type checking. They can be used to constrain the types of arguments passed to a
function or to constrain the types of template arguments, making it possible to create more robust, flexible, and
efficient code. Together they make it possible to implement generic algorithms that work with a wide range of types
while still ensuring that only types that meet the specified requirements can be used.

One of the most powerful features of C++ is the ability to use these features together to implement generic algorithms
that work with a wide range of types. By combining concepts, constraints, traits, and templates, it's possible to create
generic algorithms that are both efficient and easy to use.

For example, a generic algorithm that sorts a container of elements could use a concept to specify that the container
must have random access iterators, a trait to check if the container is a sorted container, and a constraint to ensure
that the type of elements in the container has a total order.

In this way, the algorithm can be implemented such that it is specialized for sorted containers, which can be sorted in
linear time, and for unsorted containers, which can be sorted in n log n time. This is a powerful technique that allows
you to write code that is both efficient and easy to use.

In conclusion, traits, templates, concepts, and constraints are powerful features in C++ that can be used together to
achieve more powerful and flexible type checking. They can be used to constrain the types of arguments passed to a
function or to constrain the types of template arguments, making it possible to create more robust, flexible, and
efficient code.

## An example with a generic sorting algorithm

Here's an example of how you might implement a generic sorting algorithm using concepts, constraints, and traits:

```c++
template <typename Container>
concept SortedContainer = requires(Container c) {
    c.begin(); c.end(); c.size();
    typename Container::value_type;
    {std::is_sorted(c.begin(), c.end())} -> std::same_as<bool>;
};

template <typename Container>
concept RandomAccessContainer = requires(Container c) {
    c.begin(); c.end(); c.size();
    {c.begin() + c.size()} -> std::same_as<typename Container::iterator>;
    typename Container::iterator;
    typename Container::value_type;
};

template <typename Container>
concept SortableContainer = RandomAccessContainer<Container> && 
                           requires(Container c, typename Container::value_type a, typename Container::value_type b) {
    {a < b} -> std::same_as<bool>;
};

template <RandomAccessContainer Container>
void sort(Container& c) {
    if constexpr(SortedContainer<Container>) {
        // Do nothing, container is already sorted
    } else {
        // Sort the container using std::sort
        std::sort(c.begin(), c.end());
    }
}
```

In this example, we define three concepts: `SortedContainer`, `RandomAccessContainer`, and `SortableContainer`. The
`SortedContainer` concept is used to check if the container is already sorted, the `RandomAccessContainer` concept is
used to check if the container has random access iterators, and the `SortableContainer` concept is used to check if the
container has random access iterators and the elements have a total order.

We then define a function `sort` that takes a container as its argument. The function uses the `if constexpr` statement
to check if the container is already sorted. If it is, the function does nothing, otherwise it sorts the container using
`std::sort`.

By using concepts, constraints and traits, we can create a generic sorting algorithm that can sort any container that
meet the requirement of the concepts, while ensuring that the algorithm can't be called on containers that don't have
the required properties.

It's worth noting that the example is a simplified one and a more robust implementation would include more error
handling and additional features like sorting in descending order.