{{#template name="tutorials.whatsapp2.meteor.step_02.md"}}

Now that we're finished with the initial setup, we can start building our app.

An Ionic application is made out of pages, each page is an Angular component.

## First page

Let's create the first page and call it `TabsContainer`:

{{> DiffBox tutorialName="whatsapp2-meteor-tutorial" step="2.1"}}

We defined 3 tabs (see [documentation](http://ionicframework.com/docs/v2/api/components/tabs/Tabs/)): `chats`, `contacts`, `favorites`. 
In this tutorial we want to focus only on the messaging system, therefore we only gonna implement the chats tab, the rest is just for the layout.

Now we need to include this component in the `AppModule` to make it available for our application:

{{> DiffBox tutorialName="whatsapp2-meteor-tutorial" step="2.2"}}

## Navigation

One thing is missing and that's the root page. The application doesn't know which page to load at the beginning.

Navigation is handled through the `<ion-nav>` component. Go to AppComponent's template to change it:

{{> DiffBox tutorialName="whatsapp2-meteor-tutorial" step="2.3"}}

Now we can define `rootPage` and use `TabsContainerComponent`:

{{> DiffBox tutorialName="whatsapp2-meteor-tutorial" step="2.4"}}

Navigation in Ionic works as a simple stack. New pages are pushed onto and popped off of, corresponding to moving forward and backward in history.

## Chats

We're going to create a component that contains list of chats.

First thing, a template:

{{> DiffBox tutorialName="whatsapp2-meteor-tutorial" step="2.5"}}

Then, the actual component, called `ChatsComponent`:

{{> DiffBox tutorialName="whatsapp2-meteor-tutorial" step="2.6"}}

As you probably remember, it still need to be added to AppModule:

{{> DiffBox tutorialName="whatsapp2-meteor-tutorial" step="2.7"}}

Since, the component is available, we can bind it to Chats tab:

{{> DiffBox tutorialName="whatsapp2-meteor-tutorial" step="2.8"}}


## Theme

Ionic2 provides us with a new theming system.
The theme is determined thanks to SASS variables located in the file `client/styles/ionic.scss`.
By changing these variables our entire app's theme will be changed as well.
Not only that, but you can also add new theming colors, and they should be available on the HTML as attributes, and the should affect the theming of most Ionic elements once we use them.

Since we want our app to have a Whatsapp theme, we gonna define a new variable called `whatsapp`:

{{> DiffBox tutorialName="whatsapp2-meteor-tutorial" step="2.9"}}

Now whenever we will use it as an HTML attribute we gonna have a greenish background, just like Whatsapp.

{{> DiffBox tutorialName="whatsapp2-meteor-tutorial" step="2.10"}}


## Models

It's time to think about the data structure of chats and messages.

Let's begin with a message. It should contain content and date of creating.

{{> DiffBox tutorialName="whatsapp2-meteor-tutorial" step="2.11"}}

Because it represents a Mongo Object we also added `_id` property.

Do the same for a chat:

{{> DiffBox tutorialName="whatsapp2-meteor-tutorial" step="2.12"}}

Chat has title, picture and an object with a last message.


## Data

Whatsapp needs data, so we going to define dummy chats just so we can test our view.

{{> DiffBox tutorialName="whatsapp2-meteor-tutorial" step="2.14"}}

As you can see we're using a package called [`Moment`](http://momentjs.com/) to fabricate some dates. Let's install it:

    $ npm install moment

It requires declarations:

    $ typings install --save --global dt~moment

We used `Observable.of` that creates an `Observable` that emits values we specified as arguments.

## View

Let's update the view of ChatsComponent: 

{{> DiffBox tutorialName="whatsapp2-meteor-tutorial" step="2.15"}}

We placed two buttons at the end of Navigation Bar.
First's purpose is to add new chat, but second's to open a menu with more options.

New `ion-content` contains list of chats. Each element has a picture, title and an information about the last message.

> **NOTE:** Ionic elements will always have a prefix of `ion` and are self explanatory. Further information about Ionic's HTML elements can be found [here](ionicframework.com/docs/v2/component). It's very important to use these elemnts since they are the ones who provides us with the mobile-app look.

The `*ngFor` attribute is used for iteration and is equivalent to Angular1's `ng-for` attribute. The '*' sign just tells us that this is a template directive we're dealing with (A directive that should eventually be rendered in the view). To prevent any misunderstandings, let's have a quick example for some of the notations in Angular2's template syntax that we gonna use in this tutorial:

<table class="variables-matrix input-arguments">
  <thead>
  <tr>
    <th><strong>Template syntax</strong></th>
    <th></th>
  </tr>
  </thead>
  <tbody>
  <tr>
    <td>&lt;input [value]="firstName"&gt;</td>
    <td>Binds property `value` to the result of expression `firstName`.</td>
  </tr>
  <tr>
    <td>&lt;button (click)="readRainbow($event)"&gt;</td>
    <td>Calls method `readRainbow` when a click event is triggered on this button element (or its children) and passes in the event object.</td>
  </tr>
  <tr>
    <td>&lt;div title="Hello &#123;&#123;ponyName&#125;&#125;"&gt;</td>
    <td>Binds a property to an interpolated string, e.g. "Hello Seabiscuit". Equivalent to: &lt;div [title]="'Hello ' + ponyName"&gt;</td>
  </tr>
  <tr>
    <td>&lt;my-cmp [(title)]="name"&gt;</td>
    <td>Sets up two-way data binding. Equivalent to: &lt;my-cmp [title]="name" (titleChange)="name=$event"&gt;</td>
  </tr>
  <tr>
    <td>&lt;p&gt;Card No.: &#123;&#123;cardNumber &#124; myCreditCardNumberFormatter&#125;&#125;&lt;/p&gt;</td>
    <td>Transforms the current value of expression `cardNumber` via the pipe called `myCreditCardNumberFormatter`.</td>
  </tr>
  </tbody>
</table>

As you probably noticed, we used `AsyncPipe` to display the result of Observable under `chat` property.

Let' make it to look better by creating the `chats.component.scss` file:

{{> DiffBox tutorialName="whatsapp2-meteor-tutorial" step="2.16"}}

To include those styles in our component we need to:

{{> DiffBox tutorialName="whatsapp2-meteor-tutorial" step="2.17"}}

We also want to display date under `createdAt` property in a proper way. Moment library contains a package for Angular that will help us.

    $ npm install angular2-moment@1.0.0-beta.3 --save

It's not yet available to Whatsapp. Let's change it:

{{> DiffBox tutorialName="whatsapp2-meteor-tutorial" step="2.19"}}

Now we can use `AmCalendarPipe`:

{{> DiffBox tutorialName="whatsapp2-meteor-tutorial" step="2.20"}}

Pipes serves the same proposes as AngularJS' filters and they share exactly the same syntax, only they are called in a different name.

Last thing before we'll move to the next chapter.
Those TypeScript errors about missing module are very annoying.
Good news is, we can simply fix it by declaring a module with a wildcards!

{{> DiffBox tutorialName="whatsapp2-meteor-tutorial" step="2.21"}}

Now TypeScript expects that every file that ends with `.scss` exports `innerHTML` as a string.

This is how the chats tab should look like:

> *android* {{tutorialImage 'whatsapp2' 'screenshot-1-md.png' 500}}

> *ios* {{tutorialImage 'whatsapp2' 'screenshot-1-ios.png' 500}}

And once we slide a chat item to the left:

> *android* {{tutorialImage 'whatsapp2' 'screenshot-2-md.png' 500}}

> *ios* {{tutorialImage 'whatsapp2' 'screenshot-2-ios.png' 500}}

{{/template}}