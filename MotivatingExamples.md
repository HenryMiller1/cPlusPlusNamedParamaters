
# strong types replacement

```C++
int b = … // type could be double or some other type
int a = … // type could be double or some other type
// a lot of code using/changing a and b
point p(a /* x */, b /* y */);
```

In the case of points there is a mathematical convention of x, y – but even in this context it is useful to tell the reader who might not be familiar with the particular point class in question that the constructor is x,y and not doing some other calculation with these values. In domain specific contexts there often isn’t a strong convention and so it is useful to reminding future readers exactly what different parameters to a function are.

This example was placed first as a straw-man worthy of discussion: while named parameters do solve the readability problem having x and y being strong types are better. X and Y are conceptually different and the type system should be enlisted to ensure you don’t mix them up accidentally. In cases where you want to convert one to the other you want an ugly cast to call attention to the fact that you are doing it intentionally.

Those interested in these examples are really saying that creating a strong type is too difficult, or it imposes many changes to the rest of their code (the function should be re-written to work in terms of x_t and y_t – which might get x and y from a different domain with a different x_t/y_t that is not compatible.

The obvious alternative for a junior programmer is to write a full class for x and y, which are essentially copies of each other, and implement the some subset of interface of int, including all the operators, with all the required overloads. This is not conceptually hard, but there are plenty of opportunities for bugs. It is likely that this version would be written in such a way that a compiler will not inline the function operators thus bloating the binary size (though if either of these downsides is significant is debatable)

More expert programmers are likely come up other alternatives. The most common one is adding a tag to a template.  This works, but it is hard to explain to novices. If the compiler is good the resulting code should be fast, but the compiler needs to work so compile times are not good. This also assumes that all strong types want the same operator overloads.

P0707 (metaclasses) has an interesting way to create strong types, but it is only a couple paragraphs. This might be a good solution to the problem that is novice friendly but that remains to be seen. The author suspects that the ideal strong type needs some abilities not currently in meta classes. For example, remove the multiplication concept from int. This problem seems solvable as a separate add-on but that needs to be explored.

All of the above fail to work with the rest of C++ in general. Since non-decay to int is a goal, algorithms (e.g. std::abs) do not automatically work and users are not allowed to provide the overload required for it to work. 

> We have tried to get rid of "primitive obsession" for two decades. The workarounds entail such an amount of boilerplate or other unfortunate package deals (function-specific types in namespace scope) that users choose to live with bugs rather than introduce the workarounds. So, rather than try to force users to do something they refuse to do, which is using the current known workarounds, we can make the workaround less boilerplatey, or we can remove the one problem that always kills a named argument proposal, which is that they can't be used with standard library functions.
>Ville Voutilainen <ville.voutilainen@gmail.com>

## Recommendation
C++ should provide better abilities to create strong types. P0707 seems like the right direction but this should be explored in more detail. There may be a need for a standard meta-class around making strong types. Also, there may be a need for something to indicate that this type decays to a different type only for algorithms that take and return that same decayed type, the algorithm should return the strong type. While not all problems with strong types are [easily] solvable, things can be improved.

This may be an area for WG20 (education) to explore as well: how can we encurage new users to write and use strong types.

To encourage the above the committee should adopt a position such as: Adding named parameters to the language should not be considered until strong types work better in the language and the state of teaching is such that novices use strong types when they are appropriate. 

# Readability

```C++
point_t good_local_name(1,2);
point_t other_local_name(5,6);
// other uses of the above points
circle c2(good_local_name /* center */, other_local_name /* point on edge */);
```
Here a circle is being created from two points, but it isn’t obvious what the points mean without further comments. The name of the variable can be used in some cases, but here the points are used for other purposes unrelated to the fact that they are also used to make a circle. By using a named parameter, the user can indicate to the reader what the parameters mean, something that is not obvious in context.

This differes from the previous example in that here point is the correct type for both parameters.  A center_point and edge_point type are wrong: you are likely to construct other shapes and lines from these points.

This example is only about creating a syntax for the comment. Alone it is not motivating since a comment serves the purpose already, but it leads into the others and makes an important point: communicating purpose to the future reader is a goal.

# Detect incorrect paramaters

```C++
class circle {
circle(point point_on_edge, point center);
};

void funct() {
point good_local_name(1,2);
point other_local_name(5,6);
// other uses of the above points
circle c(good_local_name /* center */, other_local_name /* point on edge */);
}
```

The is the same as the previous example, except here there is a bug in the code. Since comments are not checked the bug could potentially go unnoticed for a long time. With a formal syntax it should be easy for a compiler to verify that the correct parameters are passed in, or fail with a useful diagnostic if the paramates are wrong.

This example can be read a different way: if the correct paramaters are passed in - but in the wrong order - the compiler should just fix it. There may be a good reason that the constructor should have one order and this use have the other. While this can create confusion, general experience with other languages suggest that the problem isn't a real worry.

# Change some defaults values

```C++
void func(enum_A first = enum_a_default, enum_B second = enum_b_default);

...
func(second = non_default_value);
```

Here the user of func wants to use the deault value for the first paramater, but a different value for the second. Currently in C++ this is only possible my looking up the the default value for the first paramter and adding it. By allowing skipping the default values it makes it clear which values are of interest because they are different.

In Python many standard library functions have a long list of paramters with default values, the defaults are generally reasonable but there is often one that is wrong for some particular application. This is controverisal in Python, it is generally agreed that so many paramaters isn't ideal, but the ability to change just the settings you care about is desired.

# obsolete named constructor idiom

```C++
class circle {
public:
circle(length_t /* radius */);
circle(length_t /* diameter */);
circle(length_t /* circumference */);
};
```

Here the code is not currently valid C++. The common work around is the name constructor idiom. While that works, many are dissatisfied with that for various reasons. Allowing code like the above would eliminate (or reduce the need) for the name constructor idiom.

# overload with the same type
Example needed.


