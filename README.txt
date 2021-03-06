Borg mods:
-added individual widget events eg.
saveBtn = new ofxUIButton("SAVE", false, dim,dim, dim,dim*4);
ofAddListener(saveBtn->widgetEvent,this,&ChordProgressorView::save);

-added individual unique ids (uid) for widgets that remain unchanging to be able to identify the sender in a global event setup with several widgets with the same name


 

//////////////////
ofxUI is an addon for openFrameworks (version 07) that easily allows for the creation of user interfaces aka GUIs. ofxUI also takes care of widget layout, spacing, font loading, saving and loading settings, and widget callbacks. ofxUI can be easily customized (colors, font & widget sizes, padding, layout, etc).

ofxUI contains a collection of minimally designed graphical user interface (GUI) widgets (Buttons, Toggles, Image Buttons, Label Buttons, Button Matrices, Drop Down Menus, Sliders (rotary, range, vertical, horizontal), Number Dials, 2D Pads, Labels, FPS labels, and Text Input Areas). 

This library allows for rapid UI design and development. It uses ofTrueTypeFonts for nice font rendering. Widget layout is semi-automatic, and can be easily customized. ofxUI is a GL based GUI and uses openFramework's drawings calls to render its widgets. It integrates into openFrameworks projects very easily since it was designed specifically for OF. There are many examples included in the download that show how to add widgets, customize their placement, get values from different types of widgets, set widget values, add callback functions for gui events, saving and loading settings, and more.

This UI library was inspired & influenced by: 

- controlP5 (http://www.sojamo.de/libraries/controlP5/) 
- GLV (http://mat.ucsb.edu/glv/)
- SimpleGUI (http://marcinignac.com/blog/simplegui/)
- Apple's User Interface 

Additionally, I was motived to write my own since I wanted something like ControlP5 (which is easy to get up and running) for Processing for OF. I use ControlP5 a lot and love its minimal aesthetic. I also love simpleGUI for Cinder, but since I primarily code using OF, I developed ofxUI with the intentions that it needs to be minimally designed, intuitive to use, easily integrated, flexible and customizable.
ofxUI was / is developed by Reza Ali (www.syedrezaali.com || syed.reza.ali@gmail.com || @rezaali). It has been tested on OSX  and iOS (OF 07). It should work on linux and windows, since its only dependency is openFrameworks & ofxXmlSettings. ofxUI is open source under an MIT License, therefore you can use it in commercial and non-commercial projects. If you do end up using it, please let me know. I would love to see examples of it out there in the wild. If you plan to use it in a commercial project please consider donating to help support this addon and future releases/fixes/features (http://www.syedrezaali.com/blog/?p=2172).

On a personal note: I designed ofxUI so that if I present my work to a client or put out an App on the App store, it would have a decent aesthetic and be presentable to non-hackers/programmers. I really like the widgets in MaxMsp, so I wanted to have something similar so when I prototype in C++ I could easily go about creating a UI that is pleasing to use and works robustly. 

In terms of performance, I haven't compared it to other GUIs out there, but since its written using OF drawing calls and uses Listeners built in to OF, it run very fast and doesn't take a lot of CPU...atleast when I tested it when my sketches. On my laptop (Apple Macbook Pro 2009) without vertical sync the all widgets example runs upwards of ~350 fps. 

When I first started programming with C++ it was difficult for me to use the other GUIs...their either depended on other libraries, which weren't included in the download, and it wouldn't work out of the box, or they were complicated to integrate into my projects. Additionally, I was motived to write my own since I wanted something like ControlP5 (which is easy to get up and running) for Processing for OF. I use ControlP5 a lot and love its minimal aesthetic. I also love simpleGUI for Cinder, but since I primarily code using OF, I developed ofxUI with the intentions that it needs to be minimally designed, intuitive to use, easily integrated, flexible and customizable.


TUTORIAL: 

This tutorial will provide step by step instructions on how to integrate ofxUI with a project your are working on, for simplicity I will assume we are starting from an empty project. For this we are going to be assuming you are using openFrameworks 07 for OSX, however these instructions should be easily adaptable for iOS.

1. After downloading ofxUI, place it in your openframeworks addons folder. 

2. Create a new project that is an example of the openframeworks emptyExample (found in the apps/examples folder in OF).

3. Open the project in xCode. 

4. Drag the src (addons/ofxUI/src) folder within ofxUI into the addons folder in the xCode project (located on the left side of OF). You can rename this ofxUI to make things neat. 

5. When prompted "Choose options for adding these files", just press "finish." Then add ofxXmlSettings to the addons folder inside your xCode project. ofxUI uses this addon to save and load settings from XML files. 

6. Now we must import some resources that ofxUI will use into the data folder of the project your are working on. The easiest way is to go to one the examples in the ofxUI addon folder and copy the bin/data/GUI folder into your project's data folder "bin/data/".

Note: The last step is critical so ofxUI can find the proper resources for rendering font. 

7. Now go back to xCode and include "ofxUI.h" in your testApp.h file.

8. Then in your testApp.h file, create a new ofxUICanvas object by typing "ofxUICanvas *gui;" within the testApp class. In addition create two functions:

void exit(); 
void guiEvent(ofxUIEventArgs &e);

9. Switch over to your testApp.cpp file and define the exit and guiEvent functions: 

void testApp::exit()
{
	
}

void testApp::guiEvent(ofxUIEventArgs &e)
{
	
}

10. Within the setup function we are going to initialize the gui object and add widgets to it. So one way to do that is: 

gui = new ofxUICanvas(0,0,320,320);		//ofxUICanvas(float x, float y, float width, float height)		

Note: The arguments define the GUI's top left corner position and width and height. If no arguments are passed then the GUI will position itself on top of the window and the back of the GUI won't be drawn. 

11. In the exit function we have to delete the gui after we are done using the application. But before this we want to tell the GUI to save the current values in the widgets to an XML file. Thus your exit function should look like: 

void testApp::exit()
{
    gui->saveSettings("GUI/guiSettings.xml");     
    delete gui; 
}

12. We are now going to add widgets to the GUI: 

gui->addWidgetDown(new ofxUILabel("OFXUI TUTORIAL", OFX_UI_FONT_LARGE)); 
gui->addWidgetDown(new ofxUISlider(304,16,0.0,255.0,100.0,"BACKGROUND VALUE")); 
ofAddListener(gui->newGUIEvent, this, &testApp::guiEvent); 
gui->loadSettings("GUI/guiSettings.xml"); 

Note: The second to last line adds a listener/callback, so the gui knows what function to call once a widget is triggered or interacted with by the user, don't worry if its doesn't make too much sense right now, you'll get the hang of it. The last line tells the gui to load settings (widget values from a saved XML file, if the file isn't present it uses the default value of the widgets). 

13. Now to the guiEvent function, we need to react to the user input. The argument of the guiEvent function, ofxUIEventArgs &e, contains the widget which was modified. To access the widget we do the following:

void testApp::guiEvent(ofxUIEventArgs &e)
{
    if(e.widget->getName() == "BACKGROUND VALUE")	
    {
        ofxUISlider *slider = (ofxUISlider *) e.widget;    
        ofBackground(slider->getScaledValue());
    }   
}

Note: The if statement checks to see which widget was triggered. The way it does that is a string comparison. If the widget is the "BACKGROUND VALUE" slider widget, then case the widget to a slider and retrieve its value by the "getScaledValue()" function. 

Note: A more practical example might save the value from the slider and use it else where the program. 

14. Lets add a Toggle to toggle the window between fullscreen and window mode. In the setup function add another widget after the other widgets: 

gui->addWidgetDown(new ofxUIToggle(32, 32, false, "FULLSCREEN"));

Note: if we placed this function before the other addWidgetDown calls then the Toggle would be placed above the slider and able. Lets keep things neat and put in under the slider. 

15. We have to now respond to the "FULLSCREEN" toggle widget so we add more functionality to our guiEvent function. In the end it should look like:

void testApp::guiEvent(ofxUIEventArgs &e)
{
    if(e.widget->getName() == "BACKGROUND VALUE")
    {
        ofxUISlider *slider = (ofxUISlider *) e.widget;    
        ofBackground(slider->getScaledValue());
    }
    else if(e.widget->getName() == "FULLSCREEN")
    {
        ofxUIToggle *toggle = (ofxUIToggle *) e.widget;
        ofSetFullscreen(toggle->getValue()); 
    }    
}

Notes: So you can see adding other kinds of widgets and reacting to them are done in a very similar manner. Explore the examples to check out how to access some of the more complex widgets, I hope you'll see thats its pretty intuitive. I tried my best to limit the amount of code that needs to be written, however I kept it open enough so people can be expressive with it. 

If you lost your way somewhere in the tutorial, don't worry the whole project is included in the examples in the ofxUI addon folder!

NOTES:

If you don't need to save/load the gui settings and don't want to include ofxXmlSettings in your project, you can set the following define in your project build settings: OFX_UI_NO_XML