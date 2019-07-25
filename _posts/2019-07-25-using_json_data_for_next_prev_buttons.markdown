---
layout: post
title:      "Using Json Data For Next/Prev Buttons "
date:       2019-07-25 13:48:03 -0400
permalink:  using_json_data_for_next_prev_buttons
---

My latest update to CORE, a course creation rails application, was made to fit requirments where JavaScript, JSon, JQuery, and the serializing of data where to be used. This was exciting because I would now be making my application more dynamic and modern, it would come with challenges but I was eager to get started. Quickly challenges hit me, especially because I wanted to do things that I hadn't even learned yet or didn't have such a strong grasp on. The most challenging being the creation of a next button that would allow the user to go through the courses without a page refresh. I quickly realized there weren't many thorough guides on building a JavaScript function that accomplished this task. With the help of my instructurs and scaling back I was able to accomplish this goal. With this new information, I thought it would be good to give other people my outline on how I created the next button using JavaScript and Json data. 

### Create A Link/Button on View Page
Since I wanted to click through all the courses created on the application I choose to put my link, using an href tag, in the Show page of my Courses.
```
<a href="#" class="js-next" data-id="<%= @course.id %">  Next Course </a>
```
Making sure to give my link a Class and setting my "data-id" equal to the course id that my show page was currently displaying.

### Assign a Class to HTML Elements
On the same view page as above, assign classes to all the html elements that you want to be displayed on the page without refreshing.
```
<h1 class="courseTitle"><%= @course.title %> Course<h1>
<h3 class="courseCategory">Category: <%= @course.category %> </h3>
<h4 class="courseTime">Time that should be spent on course: <%= @course.time %> Minutes </h4>
<p class="courseDescription"><%= @course.description %></p>
```

When choosing class names, make the names deliberately so others can also make sense of the titles you choosen. Besides the addition of your link your page should look exactly the same before you added classes.

### Adding To Your Manifest
The next step is creating a JavaScript funciton in your manifest file and using JQuery to grab the link we created on the view page. After succusfully selecting the link using our assigned class ".js-next", we want to let the function know to use our script once the "click" action has taken place on the link.

Next we prevent the default action of the link, and create a variable that parses and adds 1 to our "data-id" attribute that was previously assigned.

If you are using ES6 we can use an arrow function. First we want to prevent 
```
const nextWorker = () => {
    $(".js-next").on("click", function(e) {
        e.preventDefault();
        let nextId = parseInt($(".js-next").attr("data-id")) + 1;
 });
}
```

### Using A Get request
Next we will access our Json by using a get request `$.getJSON("url", function())`

Make sure to pay close attention to how you construct your URL, "/courses/" + nextId, should resemeble your URL *http://localhost:3000/courses/5*. Remember since there is no page refresh your URL will NOT change after hitting your next link, just the information on the page.

The use of selectors inside our function will allow us to grab the elements we want and replace it with the data that is on the *next* page. `$(".courseTitle)` grabs the Title element I assigned the class to. `.text(data["title"])`  gets the texts contents from the element we've accessed and places it inside the element we got using our JQuery selector.
```
const nextWorker = () => {
    $(".js-next").on("click", function(e) {
        e.preventDefault();
        let nextId = parseInt($(".js-next").attr("data-id")) + 1;
            $.getJSON("/courses/" + nextId,  function(data){
                $(".courseTitle").text(data["title"]);
                $(".courseCategory").text("Category: " + data["category"]);
                $(".courseTime").text("Time that should be spent on course: " + data["time"] + " Minutes");
                $(".courseDescription").text("Description: " +  data["description"]);
  });
 });
}
```

### Setting The Link
Our last step is set our link to the new id so when the link is clicked *again* it will show the next pages info and so on.

Again we're grabbing the link using our selector, accessing the "data-id" attribute and setting it to the data id of the information we are currently looking at. 
```
$(".js-next").attr("data-id", data["id"]);
```

Go ahead and click your next link. Your mind has offically been blown, you're welcome.

<iframe src="https://giphy.com/embed/5i7umUqAOYYEw" width="480" height="327" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/firefly-dodgeball-wash-5i7umUqAOYYEw">via GIPHY</a></p>


Finally, we've created a succusful next button....or have we? This function works when the id's are in succession to each other, so if it goes from 4 and jumps to 8 because of delted posts/courses then it becomes a problem. I hope to go into more detail on how we can avoid these pitfuls in my next blog post. Untill then I hope this has helped in the basic understanding of how to create a next link. 





