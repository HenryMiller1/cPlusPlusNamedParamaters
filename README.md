# cPlusPlusNamedParamaters

## Introduction 

Every few months on the c++ std-proposals mailing list someone will write a proposal for how to do named parameters, in most cases this focuses on syntax without any other motivation. Discussion quickly follows three different tracks: bikeshed about the syntax, discussion on why the proposed syntax cannot work in some obscure cases, or discussion on the usecase. Generally, discussion dies off without resolution and the submitter goes away disappointed.

After reading several different discussions there is agreement that we first need agreement on use cases. It turns out that there are many different use cases that people have for them. Some of the use cases solve real problems with C++, while others are things that should be discouraged.

The is an attempt to get input on from the committee on where C++ should be improved to solve the problems presented. No solution is presented just problems to see where the status quo is considered sufficient, and where the language should be improved for the better.  

Note that named parameters is the general proposed solution for these problems, a solution that has been well explored in other languages. However, that should not be taken as precluding some other solution if a better one can be imagined.

# goals

1. Make it easier to write clear and correct code
1. Not change ABI for existing code
1. Support future expansion

Make sure it is worth the trade offs

# Process
The following things need to be done before this can get anywhere

1. [] Get a list of everything named paramaters can do, list alternatives and why they are better/worse
1. [] Direction from Evolution working group on what they think is useful
1. [] Start writing proposals that address the above
