
# FrntEnd Developer Technical Interview Exercise #1

![Lets Code](https://media2.giphy.com/media/qgQUggAC3Pfv687qPC/giphy.gif?cid=ecf05e47hztiigl4qlnua6k07hsp6fj0f4t5tewihuo39zmm&rid=giphy.gif&ct=g)

This is a technical interview exercise for a front-end developer role that I was tasked with, which involves constructing a to-do list application utilizing HTML, CSS, and JavaScript. The task was to be completed within a designated timeframe and presented several potential errors. While it is uncertain if the errors were intentional, I was able to identify and rectify some of them, but not within the desired timeframe of the interview which lasted a duration of 60 mins. 

-The initial five minutes of the interview were dedicated to introductory conversation between myself and the interviewer. 
-Following this, 30 mins were allocated towards analyzing the code structure to effectively address Task 1. 
-Despite investing 20 additional mins into resolving the code in multiple ways, the task was not successfully completed in full. 
-The remaining 5 mins of the interview allowed for questions as I continued to code and finalize the task. Subsequent to the interview, I dedicated an additional 3-4 hours of independent work to refine and finalize the solutions.

For the purpose of review and discussion, I will outline the steps I took to address the issues. In terms of the application structure, I would have implemented it differently than the current setup. The tasks assigned were as follows:

## Tasks

1.  **Fix not being able to add todos:** Typing a todo in the input and pressing Enter should add it to the list.
    
2.  **Add "{numActiveTodos} item(s) left" status in footer:** The number is a count of to-dos that have not been completed. The positioning should be similar to the static image. It doesn’t have to be pixel-perfect.
    
3.  **Add "Clear completed" button in footer:** Clicking the button removes all the completed todos. The button is right-aligned and vertically centered inside the footer. The button is ONLY VISIBLE if there are completed todos, otherwise invisible. Style the button so it appears the same as in the static image.
    
4.  **Add item filtering (e.g. all, active, completed) in footer:** Filters are placed in the footer, between the active item label on the left and the clear completed button on the right. Clicking a filter selects it and filters the list of todos. Style the filter buttons so they show a light gray border on hover and a light gray background color when selected.

## Answers

### Task #1 - Fix not being able to add todos

To fix the issue where the user is unable to add new todos to the list, we need to add an event listener to the input field that listens for the "keydown" event. When the user presses the "Enter" key, we should retrieve the value of the input field, create a new todo object, and add it to the list of todos. Here is the implementation:
```js
const input = document.querySelector('.new-todo');

input.addEventListener('keydown', (event) => {
  if (event.key === 'Enter') {
    const todoName = input.value.trim();
    if (todoName) {
      const todo = { name: todoName, completed: false };
      store.add(todo);
      renderApp();
      input.value = '';
    }
  }
});
```

This code adds an event listener to the input field that listens for the "keydown" event. When the "Enter" key is pressed, we retrieve the value of the input field and create a new todo object. We then add the new todo to the list of todos, re-render the app, and reset the input field.

-----

### Task #2 - Add "{numActiveTodos} item(s) left" status in footer
-----

To add the "{numActiveTodos} item(s) left" status to the footer, we need to count the number of active todos (i.e., todos that are not completed) and display it in the footer. Here is an example of what that looks like:

```js
function renderFooter() {
  const id = 'footer-template';
  const data = {
    hideFooter: !store.hasTodos() ? 'hidden' : '',
    numActiveTodos: store.getActiveTodos().length,
  };
  const footer = Framework.templateToElement(id, data);
  return footer;
}

```
This possible solution above uses the `getActiveTodos()` method to count the number of active todos and stores it in the `numActiveTodos` variable. We then add this variable to the data object that is passed to the footer template and render the footer. Technically applying this method should work.

________
### Task #3 - Add "Clear completed" button in footer

To add the "Clear completed" button to the footer, we need to check if there are any completed todos and only display the button if there are. Here is an example of what that might look like:
```js
function renderFooter() {
  const id = 'footer-template';
  const data = {
    hideFooter: !store.hasTodos() ? 'hidden' : '',
    numActiveTodos: store.getActiveTodos().length,
    hasCompletedTodos: store.getCompletedTodos().length > 0,
  };
  const footer = Framework.templateToElement(id, data);
  const clearCompletedButton = footer.querySelector('.clear-completed');

  if (data.hasCompletedTodos) {
    clearCompletedButton.style.display = 'block';
    clearCompletedButton.addEventListener('click', () => {
      store.clearCompleted();
      renderApp();
    });
  } else {
    clearCompletedButton.style.display = 'none';
  }

  return footer;
}
```
This code first checks if there are any completed todos using the `getCompletedTodos()` method and stores the result in the `hasCompletedTodos` variable. If there are completed todos, we display the "Clear completed" button, add an event listener to it that calls the `clearCompleted()` method to remove all completed todos from the list, and re-render the app. If there are no completed todos, we hide the "Clear completed" button.

### BONUS: Task #4 - Add item filtering (e.g. all, active, completed) in footer

I attempted to add item filtering to the footer. The first step we would need to do, is create three filter buttons (for "All", "Active", and "Completed" todos) and add event listeners to them that filter the list of todos based on their completion status. Take a look at what I did here:

```js
function renderFooter() {
  const id = 'footer-template';
  const data = {
    hideFooter: !store.hasTodos() ? 'hidden' : '',
    numActiveTodos: store.getActiveTodos().length,
    hasCompletedTodos: store.getCompletedTodos().length > 0,
  };
  const footer = Framework.templateToElement(id, data);
  const filterButtons = footer.querySelectorAll('.filter-button');

  filterButtons.forEach((button) => {
    button.addEventListener('click', () => {
      const filter = button.dataset.filter;

      switch (filter) {
        case 'all':
          store.setFilter(null);
          break;
        case 'active':
          store.setFilter(false);
          break;
        case 'completed':
          store.setFilter(true);
          break;
      }

      renderApp();
    });
  });

  return footer;
}
```
The code above (which would be the possible solution for task 4) creates three filter buttons with the `filter-button` class and sets their `data-filter` attributes to "all", "active", and "completed". We then add event listeners to these buttons that call the `setFilter()` method with the appropriate filter value (null for "All", false for "Active", and true for "Completed") and re-render the app. The `setFilter()` method updates the `activeFilter` property of the store, which is used to filter the list of todos in the `renderList()` function.

____

## FINAL THOUGHTS
When tasked with problem-solving, it is crucial to acknowledge that even the most trivial problem necessitates careful contemplation and consideration. In my professional work as a Senior Front-End engineer, I prefer to take the time to carefully evaluate the problem at hand before attempting a solution. Rushing through a problem-solving process often results in suboptimal solutions, as well as potential errors and mistakes.

As such, my approach to problem-solving is not characterized by quick-fix solutions or hasty implementation. Instead, I take the time to analyze the issue, consider various options, and carefully evaluate each potential solution before proceeding. This process often involves rolling back previous attempts and making necessary adjustments to arrive at a well-considered, effective solution.

By taking the time to evaluate the problem and carefully consider each potential solution, I am able to deliver high-quality, reliable results that meet the specific needs of the task at hand. Rushing through the problem-solving process often leads to incomplete or erroneous solutions, which can ultimately hinder the success of a project. If this process were an attempt to emulate a typical workday, the timeframe isn't ideal.

When evaluating candidates through a pair test, I recognized that there may not always be a clear-cut answer of whether the candidate passed or failed the test. The stressful nature of the exercise can affect a candidate's performance, potentially hindering their ability to showcase their full potential. As such, it is crucial to approach the evaluation process with a comprehensive understanding of the limitations of this method and the potential impact it may have on candidate performance.

It is not uncommon for candidates to overestimate the degree of realism inherent in the simulation, leading to potentially biased or misleading evaluations of their abilities. A "best-case scenario" result in which a candidate successfully completes the exercise and provides all correct answers does not necessarily equate to a guaranteed successful hire. It is important to consider various factors such as prior experience, communication skills, and ability to work within a team dynamic in order to make a well-informed hiring decision.



## Alternate ways tech companies can approach candidates:


In lieu of simulating a working environment for candidates, it would be better to provide them with an authentic, real-world work experience. While it may not be feasible to invite candidates to work alongside the team for a week on live projects, organizations can host assessment days or hackathons to provide candidates with a meaningful, hands-on experience.

This approach offers several benefits over traditional interview methods. Firstly, it provides candidates with a tangible understanding of the work environment, the team dynamics, and the specific tasks and challenges of the role. This increased level of transparency helps candidates to make a more informed decision regarding their fit within the organization.
