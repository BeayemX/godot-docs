.. _doc_visual_script:

Visual Scripting
================

Introduction
------------

Visual Scripting is always a controversial topic, surronded by many misunderstandings.
Programmers often think the aim of it is replacing their beloved code, so they
act defensively against it. Artists and game designers think that, with it,
they can write any game they want with little effort without the need of programmers.

This situation is the result of really bad marketing from many products over time 
which, in their aim to capure the interest of artists and game designers, also
made programmers feel like they are being replaced by visual tools.

Truth is mostly, somewhat, in the middle. First of all, programmers can rest
assured that Visual Scripting in no way makes code obsolete. Artists and game
designers can also rest assured that they will be able to do a lot of things
without programming.

Visual Scripting is a tool designed to make the entry barrier to programming
much lower. As code is more visual, it needs less abstract thinking to be
understood. Any artist, animator, game designer, etc. can look at it and quickly
grasp the flow of logic.

The reason it does not make existing programming obsolete is, simply, that it does not scale as well.
It takes considerably more time to create code with it, and it's often more difficult
to modify than just writing a few characters.

With the misunderstanding cleared up, the question that remains is what are the practical
uses is for Visual Scripting.

The most common use cases are are as follows:

* Beginners into game development who want to learn an engine but have no programming experience yet.
* Artists and Game Designers who have no experience in programming and want to create quick prototypes or simple games.
* Programmers working in a team that want to make part of the game logic available to Artists or Game Designers in order to offload some of their work.

These scenarios are far more common than one might think, so this is why Godot has added this feature.

Visual Scripting in Godot
-------------------------

As with everything in Godot, we prioritize a good experience over copying or integrating third party solutions 
which might not fit nicely in the current workflow. This led us to write our own version of how we believe
this feature would work best with the engine.

In Godot, a Visual Script fits smoothly together with regular scripts in the Editor tab

.. image:: /img/visual_script1.png

In fact, Visual Scripting integrates so well to Godot that It's hard to believe it was added only
in version 3.0. This is because, when editing, the rest of Godot panels and
docks act like a palette from where you can drag and drop all sorts of information to the script canvas:

.. image:: /img/visual_script2.png

Creating a Script
-----------------

Creating scripts works the same as with other scripting languages: Just select any node in the scene
and push the "New Script" button at the top right corner of the Scene Tree dock:

.. image:: /img/visual_script3.png

Once it opens, the script type "Visual Script" must be selected from the drop down list. The script extension
must be ".vs" (for Visual Script!).

.. image:: /img/visual_script4.png

Finally, the script editor will open, allowing to start the editing of the visual script:

.. image:: /img/visual_script5.png


Adding a Function
-----------------

Unlike other visual scripting implementations, Visual Scripting in Godot is heavily based on functions.
This happens because it uses the same interface to communicate with the engine as other scripting engines.
In Godot, the scripting interface is universal and all implementations conform to it.

A function is an individual canvas with nodes connected.

A single script can contain many functions, each of which will have a canvas of it's own. This makes scripts
more organized.

There are three main ways to add functions in a script:

Overriding a Virtual Function
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Most types of nodes and other types of objects in Godot contain virtual functions. These are functions that
will be called (run your code) when something happens and can be looked up in the reference. Virtual functions
are listed when pressing the "Override" icon in the member panel:

.. image:: /img/visual_script6.png


In the following example, a function will be executed when the node is loaded and added to the running scene.
For this, the _ready() virtual method will be overriden:

.. image:: /img/visual_script7.png


Finally, a canvas appears for this function, showing the override:

.. image:: /img/visual_script8.png

As some functions expect you to return a value, they will also add a return node where such value is supposed to be
provided

.. image:: /img/visual_script9.png

Connecting a Signal to a Function
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Nodes in a tree emit signals when something happens. Godot uses signals for all sorts of things.
A typical example would be a button that emits a "pressed" signal when actually pressed.

For this, a node must be selected and the Node tab opened. This will allow inspecting the signals.
Once they are displayed, connect the "pressed" signal:

.. image:: /img/visual_script10.png

This will open the connection dialog. In this dialog, we must select the node where the signal be
connected to, and the function that will receive the signal:

.. image:: /img/visual_script11.png

If this is done right, a new function will be created in our script and a signal will automatically be
connected to it:


.. image:: /img/visual_script12.png

Creating a Function Manually
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The last way to create functions is to do it manualy. In general this is not as common unless you
really need it. Custom functions work when another (or the same) script calls them manually.
The main use case for this is to separate a function into more, or reusing your visual code.

To create a function manually, push the big "Plus" button, and a new function will be added
with a default name:

.. image:: /img/visual_script13.png

This will add a new function, which can be renamed by simply double clicking it's name:

.. image:: /img/visual_script14.png

To edit the "arguments" this function can get (the values you pass to it when you call this function),
simply click the Function node and check the inspector:

.. image:: /img/visual_script15.png

More on that will be explained later in this document.

Nodes and Terminology
----------------------

Before continuing, it must be noted that the *Node* termonology needs to be used with care. 
When refering to *Visual Script Nodes* (or generally just *Nodes*) this text will refer to the little boxes you connect with lines, 
which are part of a graph. When refering to just *Scene Nodes*, it is implied that the elements
 that make up a Scene are being refered, which ar part of a tere. Their naming is similar, but their function is different.
When refering to *Node* here, it will be implied that a *Visual Script Node* is refered to unless indicated otherwise.

.. image:: /img/visual_script16.png


Node Properties
---------------

Like in most visual scripting implementations, each node has editable properties. In Godot, though, we try to avoid
bloating the nodes with editable controls for the sake of readability. 

Nodes still display the required information as text, but editing is done via the *Inspector*. To edit them, just
select andy node and edit it's properties in the *Inspector*.

Ports and Connections
--------------------

Programming in Godot Visual Scripting is done via *Nodes* and *Port Connections* inside each function. 

Ports
~~~~~

Nodes in Godot Visual Scripting have *Ports*. These are endpoints that appear to the 
left and right of nodes and which can be used to make *Connnections*:
There are two types of *Ports*: *Sequence* and *Data*.

.. image:: /img/visual_script17.png

*Sequence Ports* indicate the order in which operations are executed. 
Typically when a *Node* is done processing, it will go to the next node from one of the ports at the right. 
If nothing is connected the function may end, or another output *Sequence Port* might be tried (this depends on the node). 
Thanks to this, it's easy to understand the logic within a function by just following the white lines.
Not every *Node* has *Sequence Ports*. In fact, most do not.

*Data Ports* ports contain typed values. Types can be any regular Godot types, 
such as a boolean, an integer, a string, a Vector3, an array, any Object or Scene Node, etc. 
A *Data Port* on the right side of a node is considered an output, while, 
a port on the left side is an input. Connecting them allows to transfer information from a node to the next. 

Not all *Data Port types are compatible and will allow connections, though. 
Pay special attention to colors and icons, as each type has a different representation:

.. image:: /img/visual_script18.png


Connections
~~~~~~~~~~~

Connecting is a relatively simple process. Just drag an *Output Port* towards an *Input Port*. 

.. image:: /img/visual_script_connect.gif

Disconnecting takes a bit more practice. Disconnecting in *Data Ports* happens by 
dragging the *Input* away, while for *Sequence Ports*, this happens by dragging the *Output* away.

.. image:: /img/visual_script_disconnect.gif

This may seem strange at the beginning, but it happens because *Data Ports* are 1:N 
(A single output port can connect to many inputs), while *Sequence Ports* are N:1 
(Many sequence outputs can be connected to a single input).

Connecting to empty space (drag to connect but unpress over empty space) is also context sensitive, it will supply
a list of most common operations. For sequences, it will be conditional nodes:

.. image:: /img/visual_script52.png

While, for data, a contextual set/get/call menu will open:

.. image:: /img/visual_script53.png


Adding Nodes
------------

Finally! We got to the fun part! But, before explaining in more detail what each type of node does, 
let's take a short look at how nodes are most commonly added and dealt with.


Accessing Scene Nodes
~~~~~~~~~~~~~~~~~~~~~

One of the most common tasks is accessing Scene Tree Nodes (again, not to mistake with *Visual Script Nodes*).
Dragging from the Scene Tree and dropping into the canvas, by default, will ask you to *call a method* (sometimes refered to as *member function*) on
this node. 

.. image:: /img/visual_script19.png

While accessing properties is desired in most cases (more on that below), sometimes *calling methods* can be useful too.
Methods execute specific actions on objects. In the above case, the mouse pointer can be warped to a position in local
coordinates to the control. Another common use case is queueing a node for deletion, which is done with the *queue_free* method.

.. image:: /img/visual_script20.png

Care must be taken that this only works if the scene being edited contains your *Visual Script* in one of the nodes! Otherwise, a warning will be shown.

Accessing Scene Node Properties
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This will be by far the most common way to *Scene Nodes* in Visual Scripting. Simply select a *Scene Node* from the *Scene Tree*,
go the the inspector, find *The Name* of the property you want to edit (hint, *not* the value!) and drag it to the canvas:

.. image:: /img/visual_script21.png

The result is that this value can be changed from your script by writing to a *Data Port*.

If instead reading this value is desired, just drag the node again but hold the *Control* key (or Command on Mac). This will create a getter:

.. image:: /img/visual_script22.png

In this case, the value can be read from a *Data Port*.


Variables
~~~~~~~~~

Variables are memory containers local the script, which can hold a value. This value can be read from any of the functions of the script, or
from other scripts via the method described in the previous step.

Adding a Variable is simple, just push the "Plus" Button on the *Variables* section of the members panel. Afterwards, doubleclick the
new variable to rename it:

.. image:: /img/visual_script23.png

Right clicking the variable allows to configure the type, as well as other properties:

.. image:: /img/visual_script24.png

.. image:: /img/visual_script25.png

As it can be seen above, the type and initial value of the variable can be changed, as well as some property hints (@TODO, document this).
Ticking the "Export" options makes the variable visible in the property editor when selecting the node. This makes it available also to 
othe scripts via the method described in the previous step.

.. image:: /img/visual_script28.png

To use the variable in the script, simply drag it to the canvas to create a getter:

.. image:: /img/visual_script26.png

Likewise, hold *Control* (*Command* on Mac) to drop a setter:

.. image:: /img/visual_script27.png


Signals
~~~~~~~

In the *Connecting Signals to a Function* item we have already learned about *Signals*. It is also possible to create your own
signals in a script and use them. For this, just do the same steps you did for variables in the previous step, except for *Signals*:

.. image:: /img/visual_script29.png

Signal can also be edited via right click menu to customize arguments:

.. image:: /img/visual_script30.png

The signal you have just created will also show together with the other node signals, this allows to eventually connect to it from another script
from another *Scene Node*:

.. image:: /img/visual_script31.png

Finally, to emit the signal, simply drag it to the canvas:

.. image:: /img/visual_script32.png

Remember that emitting a signal is a sequenced operation, so it must come from a Sequence port.


Adding More Nodes
-----------------

Now that the basics are covered, let's discuss the large amount of utilitary nodes available for your canvas!
Below the member panel, exists the list of all available node types:

.. image:: /img/visual_script33.png


Any of them can be dragged to the scene. Remember that, unlike the nodes previously discussed (e.g. dragging a property
from the inspector sets the context to the node being edited automatically), these are
added without any "contextual" information, so this has to be done manually.

.. image:: /img/visual_script34.png


Remember that you can check the class reference for what each node does, as they are documented there. That mentioned,
a brief overview of node types follows:

Constants
~~~~~~~~~

Constant nodes are nodes that provide values that, while not changing over time, can be useful as reference values. 
Most of the time they are integer or float.

.. image:: /img/visual_script36.png

Of interest here are mainly three nodes. The first one is "Constant" which allows to select any value of any type as constant,
from an integer (42) to a String ("Hello!"). In general this node is not used that often because of default input
values in *Data Ports*, but it's good to know it exists.

The second is the GlobalConstant node, which contains a long list of constants for global types in Godot. In there
you can find some useful constants to refer to key names, joystick or mouse buttons, etc.

The third one is MathConstant, which provides typical mathematical constants such as PI, E, etc.

Data
~~~~

Data nodes deal with all sorts of access to information. Any information in Godot is accessed via these nodes, so
they are some of the most important ones to use and pretty diverse.

.. image:: /img/visual_script37.png

There are many types of nodes of interest here, so a short attempt to describe them will follow:

Action
^^^^^^

Action nodes are vital when dealing with input from a device. You can read more about actions in the (@TODO ACTION TUTE LINK).
In the following example below, the control is moved to the right when the "move_right" action is pressed.

.. image:: /img/visual_script38.png


Engine Singleton
^^^^^^^^^^^^^^^^

Engine singletons are global interfaces (meaning they can be accessed without a reference, unlike Scene Nodes, they are always available).
They have several purposes, but in general they are useful for low level access or OS-Related access.

.. image:: /img/visual_script39.png

Remember that dragging a connection to empty space will help you call functions or set/get properties on these:

.. image:: /img/visual_script40.png

Local Variables
^^^^^^^^^^^^^^^

These are nodes you can use as temporary storage for your graphs. Just make sure they all have the same name and type when using them
and they will reference the same piece of memory.

.. image:: /img/visual_script41.png

As it can be seen above, there are two nodes available: A simple getter, and a sequenced getter (setting requires a sequence port).


Scene Node
^^^^^^^^^^

This is just a reference to a node in the tree, but it's easier to use this node by just dragging the actual node 
from the scene tree to the canvas (this will create it and configure it).

Self
^^^^

In some rare ocassions, it may be desired to pass this Scene Node as argument. 
It can be used to call functions and set/get properties, but it's easier to just 
drag nodes (or event he node itself that has the script) from the Scene Tree to the canvas for this.

SceneTree
^^^^^^^^^

This node is similar to the Singleton node because it references the SceneTree, which contains the active scene.
SceneTree, however, only works when the node is sitting in the scene and active, otherwise accessing it will
return as an error.

SceneTree allows for many low level things, like setting stretch options, calling groups, make timers, or even
load another scene. It's a good class to get familiar with.

Preload
^^^^^^^

This does the same function as preload() in GDScript. It maintains this resource loaded and ready to use. Rather than
instancing the node, it's simpler to just drag the desired resource from the filesystem dock to the canvas.

Resource Path
^^^^^^^^^^^^^

This node is a simple helper to get a string with a path to a resource you can pick. It's useful in functions that
load things from disk.

Comment
^^^^^^^

A Comment node works as a node you can resize to put around other nodes. It will not try to get focus or be brought
to top when seleting it. It can also be used to write text on it.

.. image:: /img/visual_script42.png

Flow Control
~~~~~~~~~~~~

Flow control nodes are all sequenecd, and allow the execution take different branches, usually depending on a
given condition.

.. image:: /img/visual_script43.png

Condition
^^^^^^^^^

This is a simple node that checks a bool port. If true, it will go via the "true" sequence port. If false,
the second. After going for either of them, it goes via the "done" port. Leaving sequence
ports disconnected is fine if no all of them are used.

Iterator
^^^^^^^^

Some data types in Godot (ie, arrays, dictionaries) are iterable. This means that a bit of code can run
for each element that it has.

The Iterator node goes through all elements and, for each of them, it goes via the "each" sequence port,
making the element available in the "elem" data port. 

When done, it goes via the "exit" sequence port.

Return
^^^^^^

Some functions can return values. In general for virtual ones, Godot will add the Return node for you.
A return node forces the function to end.

Sequence
^^^^^^^^

This node is useful mostly for organizing your graph. It calls it's sequence ports in order.

TypeCast
^^^^^^^^

This is a very useful and commonly used node. You can use it to cast arguments or other objects
to the type you desire. Afterwards, you can even drag the obj output to get full completion.

.. image:: /img/visual_script55.png

It is also possible to cast to a script, which will allow to complete script properties and functions:


.. image:: /img/visual_script54.png

Switch
^^^^^^

The Switch node is similar to the Condition node, but it matches many values at the same time.

While
^^^^^

This is a more primitive form of iteration. "repeat" sequence output will be called as long as
the condition in the "cond" data port is met.

Functions
~~~~~~~~~

Functions are simple helpers, most of the time deterministic. They take some arguments as
input and return an output. They are almost never sequenced.

Built-In
^^^^^^^^

There is a list of built in helpers. The list is almost identical to the one from GDScript (@TODO, link to gdscript methods?)
Most of them are mathematical functions, but others can be very useful helpers. Just make sure to take a look at the list
at some point.


By Type
^^^^^^^

Those are the methods available to basic types. For example, if you want a dot-product, you can search for "dot" intead of the Vector3 category.
In most cases just search the list of nodes, it should be faster.

Call
^^^^

This is the generic calling node. It is rarely used directly but by dragging to empty space on an already cofigured node.

Constructors
^^^^^^^^^^^^

These are all the functions needed to create godot basic datatypes. If you need to, for example, create a Vector3 out of 3 floats, a
constructor must be used.

.. image:: /img/visual_script44.png


Destructor
^^^^^^^^^^

This is the opposite to Constructor, it allows to separate any basic type (ie, Vector3) into it's sub-elements.

.. image:: /img/visual_script45.png


Emit Signal
^^^^^^^^^^^

Emits signals from any object.


Emit Signal
^^^^^^^^^^^

Emits signals from any object. In general not very useful, as dragging a signal to the canvas works better.


Get/Set
^^^^^^^

Generic Getter/Setter node. Dragging properties from the Inspector works better, as they appear properly configured on drop.

Wait
^^^^

The Wait nodes will suspend execution of the function until something happens (many frames can pass until resuming, in fact).
Default nodes allow you to wait for a frame to pass, a fixed frame or a given amount of time until execution is resumed.

Yield
^^^^^

This node completely suspends the execution of the script, and it wil make the function return a value that can be used to resume execution.


Yield Signal
^^^^^^^^^^^^

Same as Yield, but will wait until a given signal is emitted.


Index
~~~~~

Generic indexing operator, not often used but it's good that exists just in case.

Operators
~~~~~~~~~

These are mostly generic operators such as addition, multiplication, comparison, etc.
By default, these mostly accept any datatype (and will error in run-time if the types
feeded do not match for the operator). Is is always recommended to set the right
type for operators to catch errors faster and make the graph easier to read.

.. image:: /img/visual_script46.png


Expression Node
^^^^^^^^^^^^^^^

Among the operators, the *Expression* node is the most powerful. If well used, it allows to enormously simplify
visual scripts that are math or logic heavy. Just type any expression on it and it will be executed in real-time.

Expression nodes can:

- Perform math and logic expressions based on custom inputs (eg: "a*5+b", where a and b are custom inputs):
.. image:: /img/visual_script47.png
- Access local variables or properties:
.. image:: /img/visual_script48.png
- Use most of the existing built-in functions and available to GDScript, such as sin(),cos(),print(), as well as constructors, such as Vector3(x,y,z),Rect2(..), etc.:
.. image:: /img/visual_script49.png
- Call API functions:
.. image:: /img/visual_script50.png
- Use sequenced mode, which makes more sense in case of respecting the processing order:
.. image:: /img/visual_script51.png




