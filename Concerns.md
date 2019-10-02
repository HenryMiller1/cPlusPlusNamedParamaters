This documents general concerns that are often brought up and need to be address by any proposal. 

# Who controls the names

Do the library authors control where named parameters can be used? If not the standard library needs to specify names or we will have cases where code that works with one standard library cannot be used with a different one that picks a different name. If so there is a lot of effort required before names can be used: the entire standard library will need to be reconsidered for named paramaters meaning this feature will have slow adoption even where it otherwise makes the most sense.

Note that in the case of the standard library C++ reqires that macros not affect the library, which means most implememntations use tricks like reserved symbols for their names.  Thus proposals like "if the function signature includes the variable name for the paramater, then that is the name" will meet opposition. Also there is an argument that the variable name as used internal to a function is not always the correct name to use externally. These objections are not fatal to such proposals, but any such proposal will need to justify why the objections are worth the cost.

# std::invoke

How does this interact with std::invoke and similar user-written functions. Does the named argument pass through (how) or not?
