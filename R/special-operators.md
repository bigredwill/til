#Special Operators

I happened to encounter the `%>%` operator today while reading through some source code.  In R, anything surrounded by two `%` symbols is considered a special operator, and is essentially a function.

The special operator is used to chain commands together by passing the left hand side and right hand side to a function, which allows you to write code that is readable in the order that it executes. I think the main benefit is the avoidance of nested function calls.

_Example_ This is how we would define a 'concat' operator.
```
%.% <- function(a,b) return paste(a, b);

"a" %.% "b" == paste(a,b)

"a" %.% "b" %.% "c" == paste(paste("a", "b"), "c")
```

The library `dyplr` uses the special operator in this way.  It uses the auxillary `chain_q` function to call the functions in the parent environment, so that all of the side effects will occur in the original calling environment.

```
%.% <- function (x, y) 
{
    chain_q(list(substitute(x), substitute(y)), env = parent.frame())
}

chain_q <- function (calls, env = parent.frame()) 
{
    if (length(calls) == 0) 
        return()
    if (length(calls) == 1) 
        return(eval(calls[[1]], env))
    e <- new.env(parent = env)
    e$`__prev` <- eval(calls[[1]], env)
    for (call in calls[-1]) {
        new_call <- as.call(c(call[[1]], quote(`__prev`), as.list(call[-1])))
        e$`__prev` <- eval(new_call, e)
    }
    e$`__prev`
}
``` 


In the base library, it's used as `%*%` for matrix multiplication, `%/%` for division (though you can just use `/`), and `%in%` for checking whether a value on the left is in a vector/list on the right side.
