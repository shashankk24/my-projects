Now to pagination. While building web applications you will have to deal with presenting data to users based on relevance. So with a blog for example you want to show the most recent posts first. The sorting criteria may differ but you can’t show all the content on one page, so you split it up into pages like a book. Pagination is such a common feature in web applications that I was shocked not to find content about it which was easy to just clone into the app I was building at the time. I had to re-invent the wheel on this one. In this post I will show you how I did it. As we build I will show-off some simple but powerful concepts in Vuejs.
This post assumes that you know basic Vuejs but you should be able to follow along if you have done some JavaScript in the past. We will be displaying blog posts but we will not be building a back-end for it, thankfully JSON placeholder has our back :). JSON placeholder is a Fake Online REST API for Testing and Prototyping. We will be getting our blog posts from there and paginating them. To quickly build our page let’s get a nice template from Bootstrap Examples. The album example template works fine for me. To make this short I have already made changes to the template to fit just right. Clone this Git repo and your page should look like this:

Next, let’s bring in Vue and Axios. Axios is a JavaScript library for sending Http-requests. We will use it to get blog posts from JSON placeholder. We also need to create our own paginate.js file and link to it. This is where we will write all our Vuejs pagination code. Paste this code at the bottom of your html just before the closing body tag.

The el property defines which element our Vue app will control. This will be the main html tag to which we added an id of pagination-app. The data property contains all the data will be using in our app. posts will hold the posts we get from JSON placeholder. We will be sending our requests to the baseUrl. page is the current page being viewed, perPage defines how many posts we will be viewing per page and pages is an array of page numbers which we will populate soon.
Here’s the idea with this. When we get the posts we will display the first 9, then when a user clicks a page number we will display the posts assigned to that page number. So the first 9 posts are on page 1, the next 9 on page 2 and so on.

Notice that we are looping through displayedPosts which is a computed property we have not yet declared. Computed properties are really useful when you need to manipulate data before displaying it in the view. We are also looping through the pages array to display each page number.
Now let’s fetch the posts from JSON placeholder with Axios.

We call the getPosts function immediately our page is created using the created life-cycle hook Vuejs provides us, then once we get our posts as a response we will set them to the posts data property which we defined earlier. JSON placeholder returns 100 posts so we need to split these up into pages. The number of pages will depend on the number of posts we get back.
We will now populate the pages array with the required page numbers based on the number of posts. To do this we set up something called a watcher in Vuejs. Watchers are functions that get run whenever a data property changes. We will be watching the posts property so that as it gets set in the getPosts method it is used to populate the pages array.

We call the paginate method from the displayedPosts computed property. Earlier we bound a click event to each page number being displayed. The click event sets the page data property to the pageNumber that was clicked by the user. Because Vuejs is reactive, the displayedPosts computed property gets re-evaluated every time the page data property changes from a click event.
The paginate method slices the posts array based on the current page number and the number of posts perPage. So for page 1 it returns posts 0 to 8, for page 2 it returns 9 to 17 and so on. This way we are not directly manipulating the DOM to change the page, we are just changing the data it displays.

