#Formatting Text Output in RMarkdown  
Some tricks I learned from doing my homework assignments for Statistics entirely in RMarkdown.

####Inline R code.
Just like normal markdown, you surround the code segements with two backticks, \`. For RMarkdown to execute the code in the brackets, insert the 'r' character right after the first backtick.  
Output is \``r 2 + 2`\`.  
Output is 4.

####Rounding numbers.
 I ate \``r round(2.425637, 3)`\` sandwiches last night.
 I ate 2.425637 sandwiches last night.