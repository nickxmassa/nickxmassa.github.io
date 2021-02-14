# How Do I Make Drupal Do Things?
By now you probably know how to create a Drupal module – just create a new folder in the /modules folder, add a .info file with the same name as the folder, and you’re all set. All that is great, but what if you actually want that module to, I don’t know, DO something?

## How does Drupal know about my module?
So we mentioned briefly that you can create a module using just a folder and a .info file with a few lines of configuration. But how does Drupal know about that folder and that file?

Drupal’s startup process is known as “bootstrapping,” during which the web server receives a request for page and sends the request to Drupal, which promptly gets to work. Among the surprisingly large number of things Drupal does during bootstrapping is scans the codebase for .info files and uses that to build up the list of modules. 

There is a LOT more that happens when Drupal bootstraps, and I’m sure we’ll talk about more of the events as we explore different areas of Drupal. For now, let’s talk about how to make your module do something!

## How does Drupal activate my module?
So we now know how Drupal finds modules – by looking for them, essentially. But a module needs more than a .info file to do things on a page! How can we take code written in a .module file and make it run on the page?

Turns out the answer starts in the same place as the last one – with Drupal’s bootstrap process! As the page gets closer to being fully loaded, Drupal starts scanning the codebase for “entry points” during which it can run instructions (PHP code). These entry points can come in a number of different forms, including hooks, events, and controllers. In my opinion, hooks are the most straightforward of these, and are usable in more Drupal versions, so that’s what we’ll be focusing on for this post (don’t worry, we’ll get to the others soon enough).

### What is a Drupal hook?
To start thinking of hooks, let’s think of them in the context of creating a new node (the content type doesn’t matter). When you click “Save” on that piece of content, Drupal responds by firing off a number of instructions to prepare the site to save that node. Some examples are: 

- `Hook_entity_presave()`
- `Hook_entity_create()`
- `Hook_entity_insert()`

And these are just a few. Drupal’s API reference has the full list of entity hooks, as well as hooks provided by other modules. But let’s look at one of these in more detail, say, `hook_entity_presave()`.

The description for hook_entity_presave says it “Act[s] on an entity before it is created or updated.” What this means is if you write code in a function called `mymodule_entity_presave()`, Drupal will find that code and any other functions using that hook (remember, Drupal already found all the modules earlier on in the bootstrap process) and trigger the code to run before the node saves. Similarly, if you want code to run after a node is saved in the database, you can write it in a function called `mymodule_entity_insert()`.

And that’s it! Like we said, that’s not the only way to get Drupal to load code, but it’s a good example of how Drupal does it. Before I understood this, I simply could not understand how Drupal just “magically” knew what code to run when. Even when other developers showed me how to write hooks, it took me a long time to realize that, in a manner of speaking, Drupal triggers every hook every time it loads a page and it’ll run any code you give it as long as you tell it to do so at after certain trigger.

Now that I understand that concept, it’s easy to visualize the Drupal hook system as, well, hooks! Picture this: there’s a large track just above your head with hooks moving along a cable, and you’re holding a large loop. The hooks can take you to different locations, and when you find the one you want, you attach your loop and you’re off! Other visual metaphors include chairlifts at a ski mountain…that one might be easier to picture but I got caught up in the hook thing. 

So hopefully that helps! Please note that I intentionally did not get into how to write code that actually does things – that’s a topic for another (or hopefully several other) post. With this one I wanted to answer one of the biggest questions I had when I first started out, which was how does code go from letters on the computer to actual movement on the machine. Thanks for reading and please leave a comment if there are any additional questions – I’ll do my best to answer them or adjust the post content to better explain things.
