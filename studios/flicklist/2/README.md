---
title: "FlickList 2"
currentMenu: studios
---

Today we will add a hodge-podge of miscellaneous new features to our movie site. We'll include some CSS to apply styles to the page, and build on our interaction with the API.

Along the way, hopefully you will continue to get more comfortable with Vue, CSS, AJAX, making API calls, and working with HTML forms.

## Demo

Here is a demo of what you are trying to accomplish: [FlickList 2 Demo][demo]. Play around with the demo for a minute and get familiar with its features. You also might want to keep the demo open in a separate window, so you can refer to it while working on the assignment.

[demo]: http://htmlpreview.github.io/?https://github.com/LaunchCodeEducation/flicklist/blob/f3dae711763c73f56267ac35e076c56383183829/index.html

Note the following additions since last time:

* In the browse list, each movie is accompanied by a paragraph summarizing its plot.
* Once the user clicks the "Add to Watchlist" button for a movie, the button then becomes disabled, preventing the same movie from being added more than once.
* In the watchlist, each movie is represented by an orange rectangle. These rectangles line up next to each other from left to right, without skipping down to a new line until they run out of space on the right-hand side of their container.
* The page has some styles and is a little prettier than before.
* At the top of the browse list, there is a search bar which users can use to search for particular movies. Upon submitting the form, the browse list repopulates full of movies with matching titles.

## Obtaining the Starter Code

Same procedure as usual here. First, make sure you don't have any uncommitted changes.

```nohighlight
$ git status
On branch studio1
Your branch is one commit ahead of 'origin/studio1'.
```

Then, checkout the `studio2` branch:

```nohighlight
$ git checkout studio2
Branch studio2 set up to track remote branch studio2 from upstream.
Switched to a new branch 'studio2'
```

## A Brief Tour

Our project now looks like this:

```nohighlight
$ tree
.
├── css
│   └── styles.css
├── index.html
└── js
    └── flicklist.js
```

Notice the new directory, `css`, which contains a stylesheet, `styles.css`.

Let's look briefly at each of our files:

### index.html

The HTML file has only changed slightly since we last left it:
* In the `<head>` there is now a link to our stylesheet.
* There is a TODO asking you to add a class to watchlist items
* A TODO for adding the search form, with both a text input and a submit button
* A TODO instructing you to set the "Add to Watchlist" button to "disabled" if the movie has already been added
* And a TODO asking you to display the movie object's overview

### flicklist.js

Our javascript file is also pretty similar to last time, with just a few changes:
* There is a new function, `searchMovies`, which will be invoked as part of the submit handler on the form (see above). The body of this function is pretty empty (so far!).
* The `data` portion of your Vue will need a small change: a place to store the searchTerm your users will make in the new form.

### styles.css

Finally, open up the new stylesheet. As you can see, we've already gotten started applying some styles to the page.

As a quick exercise, go preview the page in your browser right now, and open up Dev Tools. In the Elements tab, notice that one of the things you can inspect is the styles of your elements. You can even change the styles and see the results immediately! Spend a minute playing and tinkering with the styles we have added.

## Assignment

Work your way through the TODOs in the source code. The tasks are numbered. You should work on them in the order prescribed, as follows:

### 0. API key

As usual, add your api key to the object near the top of `flicklist.js`.

### 1. Add a Description Paragraph to Each Browselist Item

On the browse list, let's spice up those list items by displaying some more data about the movies. In the html file, create a `<p>` containing a description of the movie's plot. You can find this description as a string somewhere inside the movie object. Just as the title is accessbile via the property `movie.original_title`, the description is a different property. What's the name of the property? You'll have to use a `console.log` statement to poke around and find out. (It's not `movie.description`).

### 2. Disable Buttons

Next, implement this feature: an "Add to Watchlist" button should be disabled if the movie in question is already present in the user's watchlist.

To determine whether the movie is already present, you can use the javascript array function `includes`, which returns `true`/`false` depending on whether a thing is in an array. For example:

```js
var nums = [0, 7, 5, 2];
nums.includes(5) // returns true
nums.includes(5000) // returns false
```

To disable the button, you can use Vue's `v-bind` directive. For the example below, imagine `textIsAllowed` is part of the view's data.
```html
<input type="text" v-bind:disabled="textIsAllowed" />
<!-- Or, equivalently, using Vue's shorthand for v-bind: -->
<input type="text" :disabled="textIsAllowed" />
```

Once you have this working, take a quick note of the CSS rule we used in order to achieve the visual effect. We lower the `opacity` property (in other words, transparency) of disabled buttons. In order to select for only disabled buttons, we used the `:disabled` [pseudoclass][pseudoclass].

[pseudoclass]: http://www.w3schools.com/css/css_pseudo_classes.asp

### 3. Give Watchlist Items a Class Attribute

Next, it's time to apply some styles to those watchlist `<li>`s, so that they are big orange bricks. But first, in order to do that, we'll need to give them a `class` attribute, so that our CSS can select them. Inside the HTML function, within the `v-for` iteration over `watchlistItems`, give each `<p>` tag a class of `"item-watchlist"`.

Verify that you succeeded as follows: In your browser window, add some movies to the watchlist. Then, open up the dev tools, go to the Console tab, and type this:

```js
document.querySelectorAll(".item-watchlist")
```

followed by the Enter key. You should see an array with some `<li>`s inside it, one for each watchlist item! You should not see an empty array, i.e. `[]`.

### 4. Style the Watchlist Items as Orange Bricks

Now that your watchlist items have a class attribute, you can apply styles to them. Open up `styles.css`, and create a new class selector for elements with the class "item-watchlist". Add some styles until your watchlist items resemble those orange bricks from the demo. One style you'll definitely want to apply is:

```nohighlight
display: inline-block;
```

This is what enables that left-to-right flow pattern. (See this [Stack Overflow post][so-post] for a nice overview of the differences between `block`, `inline`, and `inline-block`).

[so-post]: http://stackoverflow.com/questions/8969381/what-is-the-difference-between-display-inline-and-display-inline-block

You'll also need to apply a few other styles: a lot of padding, a little bit of margin on [just the right and bottom edges][edges], and the colors obviously need to change.

[edges]: http://stackoverflow.com/questions/356759/a-mnemonic-for-the-order-of-css-margin-and-padding-shorthand-properties

### 5. Change the Text Color to Gray

Next, create a css rule that will set a baseline default of gray for the color of all text in the body of the document. To accomplish this, you only have to make one rule! you don't have to go and start adding declarations to each and every selector in the CSS file. You simply have to apply the style to some common container, and then all descendants of the container will automatically inherit the same style.

### 6. Add a Form to the Page

The last new feature we need to add is a `<form>`, with a text field and a submit button, via which users can search for particular movies.

The first step to implementing this feature is simply to add the form to `index.html`. Go ahead and do that now. Your form does not need any of the usual attributes, like `action` or `method`, because we are going to intercept and cancel its submit event anyway. Inside the form, you should have two `<input>`s: one, a text field, and the other, a submit button.

### 7. Style the Buttons

Very briefly, let's jump back over to `styles.css` and apply some styles to the buttons, both the "Add to Watchlist" buttons and the "Search by Title" submit button on the form

Notice how the selector here includes `input[type=submit]` in order to select for both normal buttons and submit buttons on forms.

### 8. Add a Click Handler to the Form Submit

Once your form is present on the page, the next step is to give it a click handler. We want to specify that when the user presses the submit button, the `searchMovies` function (which you will implement next) gets invoked, using the search term that the user typed in.

We will use Vue's `@click` directive, which allows you to pass in some code to be executed whenever a form is submitted. You must:
* A: Add the `@click` directive, including the `.prevent` modifier, and use the `searchMovies` function as the handler.
* B: Set a `v-model` directive on your text input, so that its value will be kept in sync with data on the view. You'll need to choose a name for the data collected from that input, and add it to the `data` portion of your Vue.
* C: In the value of your `@click` handler, add an argument for the search term.

If you've done everything correctly you should be able to see some output on the console. Search for "coconut" and you should see a log statement that reads: "searching for movies with 'coconut' in their title...".

### 9. Implement the `searchMovies` Function

Finally, let's implement this function in `flicklist.js`. As a starting point, you can follow in the footsteps of the `discoverMovies` function. But a few things must be different:
* You will send the request to a slightly different url (we want to talk to a different "endpoint" on the API)
* Your `data` object must include another property, `query`, whose value will be the search term the user typed in (e.g. "coconut")


## Commit and Push to Github

When you've finished, commit your changes on Git and push them to Github.

If you run the `git status` command, you should see that you now have *unstaged* changes:

```nohighlight
$ git status
On branch studio2
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   css/styes.css
        modified:   index.html
        modified:   js/flicklist.js

no changes added to commit (use "git add" and/or "git commit -a")
```

We can stage these changes with the `add` command:

```nohighlight
$ git add --all
```

The `--all` adds all of the unstaged files, so you don't have to type them one by one.

If you check your status again now, you should see:

```nohighlight
$ git status
On branch studio2
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)
        modified:   css/styes.css
        modified:   index.html
        modified:   js/flicklist.js
```

All the files are now staged for committing. Go ahead and make a commit, using the -m flag to remind your future self (and others looking at your code) what changes you made during this commit:

```nohighlight
$ git commit -m "finish FlickList 2 studio"
[studio2 46db232] finish FlickList 2 studio
 2 files changed, 2 insertions(+), 2 deletions(-)
```

The convention is to write your commit messages using the present tense rather than past tense (e.g. "finish" rather than "finished").

If you check your status one more time, you should see this:

```nohighlight
$ git status
On branch studio2
nothing to commit, working directory clean
```

Finally, *push* your changes to your remote repo:

```nohighlight
$ git push origin studio2
Counting objects: 62, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (20/20), done.
Writing objects: 100% (22/22), 2.36 KiB | 0 bytes/s, done.
Total 22 (delta 6), reused 0 (delta 0)
To https://github.com/jharvard/flicklist.git
 * [new branch]      studio2 -> studio2
```

If you go back and revisit github.com/jharvard/flicklist, you should now see your new branch up there! Specifically, near the top-left of the screen, you should see a dropdown menu that says "Branch: master". Click that dropdown and you should see an option for "studio2". Click on that branch, and you should now see the code you just worked on.
