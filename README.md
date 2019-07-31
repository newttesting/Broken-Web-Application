# Enable and disable the Cross Site Scripting 
Edit the following line 

`/src/main/resources/templates/sixWordStories.html`

### _How to fix it:_

Simply you can use `th:text` instead of `th:utext`. `th:text` is the default behaviour of "Thymeleaf" which makes sure that text should be escaped.
