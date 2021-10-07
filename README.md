# AppNavigation

## Lesson 3: App Navigation
This is the thrid android project in [Udacity: Developing Android Apps with Kotlin course.](https://classroom.udacity.com/courses/ud9012)
- Basic of creating a fragment 
- Switching from one fragment to another using navigation actions 
- Navigation component using navigation graph
- Conditional navigation between different fragments 
- Back stack manipulation using up button
- Creating overflow menu 
- adding safe arguments 
- using intents and sharing - by creating share button on the action bar
- adding navigation drawer 
- using navigation listeners 

## Highlight for lesson 3:
### Fragments
Activity contains the fragment. Fragment needs a layout as well, on the **_onCreateView_** method. We can use
```DataBindingUtil.setContentView<ActivityMainBinding>(this, R.layout.activity_main)``` 
to set the layout in Activity. But **_setContentView_** does not work for Fragment. Instead we will be using 
```
DataBindingUtil.inflate<FragmentTitleBinding>(inflater,R.layout.fragment_title,container,false)
return binding root
```

In order, to display the fragment we can do two things add the fragment manually in the layout file of activity or create one dynamically. 
To add the fragment manually:
Steps: 
1. Open the activity layout and add
```
<fragment android:id=“@+id/name_fragment” 
android:name=“com.example.android.location_of_fragment” 
android:layout_width=“match_parent” 
android:layout_height=“match_parent”/>
```
----
### Navigation
#### Navigate:
We can navigate between different activities and fragments and activity. The previous activities are arranged in stack which is known as back stack. System back key will pop the top of the stack. Fragment Back Stack works the same, the fragments are arranged in stack as well.

#### Navigation Component:
The navigation component simplifies many navigation tasks. More importantly it helps developer to follow principle of navigation.
##### First principle: There’s Always a Starting Place (This screen should also be the last screen that the user sees after pressing the back button.)
##### Second principle: You Can Always Go Back (The navigation state of your app should be represented with a last in first out structure.)
##### Third principle: Up button goes back (mostly) If the user is at the first screen the user should not be shown the up button. 

Steps to adding nanvigation component to the project:
1. Add the ext variable within the project build.gradle
```
Build script{
  Ext{
        …
        version_navigation = ‘2.3.5’
        …
    }
}
```
In app **_build.gradle_** add the dependencies for navigation fragment ktx and navigation UI ktx. 
```
dependencies {
    ...
    implementation "android.arch.navigation:navigation-fragment-ktx:$version_navigation"     
    implementation "android.arch.navigation:navigation-ui-ktx:$version_navigation"
} 
```
2. Add the navigation graph to the project, right-click on the res directory and select **_New > Android resource_** file. Select Navigation as the resource type and give it the file name.

3. Add the Navigation Host Fragment in the Activity Layout 
```
<!-- The NavHostFragment within the activity_main layout -->
<fragment
   android:id="@+id/myNavHostFragment"
   android:name="androidx.navigation.fragment.NavHostFragment"
   android:layout_width="match_parent"
   android:layout_height="match_parent"
   app:navGraph="@navigation/navigation"
   app:defaultNavHost="true"
   />
   ```
4. Add the fragments to the Navigation Graph within the navigation editor, click the add button. A list of fragments and activities will drop down. Add the start destination fragment first and the add others.

5. Connecting the Fragments with an Action -> Begin by hovering over the first fragment. You’ll see a circular point on the right side of the fragment view. Click on the connection point and drag it to the other fragment to add an Action that connects the two fragments. 

6. To add the action to a button 
```
//The complete onClickListener with Navigation
binding.playButton.setOnClickListener { view: View ->
        view.findNavController().navigate(R.id.action_titleFragment_to_gameFragment)
}
```
#### Conditional Navigation:
A common case for conditional navigation is when an app has a different flow, whether the user is logged in or not. 
1. Add the fragment to the navigation graph that you want to navigate to.
2. Then draw two actions from the fragment to the fragment you want to navigate to.
3. Add the ```view.findNavController().navigate(R.id.action_fragment_to_another_fragment)```

----
### Back Stack Manipulation
Navigation controller handle our back stack for us. What this means is when we navigate to a destination, it’s automatically adding each destination to the back stack. 

Steps to set the back stack behavior.
1. To make action pop destinations off of the back stack on our behalf, we use the pop behavior attributes from the editor(nav graph design mode). Select the “Pop To” fragment and if you want to pop off the fragment that you pop to check the “Inclusive” check box as well. 

----
### Supporting Up Button: 
To hook up the action bar to navigation, navigation UI needs to access the navigation controller. The preferred way of finding the navigation controller in an activity is to use the Find navController method that takes the activity in the NavHostFragment ids as parameters. 
```
Val navController = this.findNavController(R.id.myNavHostFragmentName)
NavigationUI.setupActionBarWithNavController(this, navController
```
And Finally activities have **_onSupportNavigateUp_** method that our app can override. Use control + O on Android Studio to open up a window to browser through methods to override. 
 
Inside **_onSupportNavigateUp_** method, we find the navController and then we call **_navigateUp_**.
```
Override fun onSupportNavigateUp(): Boolean {
 Val navContorller = this.findNavController(R.id.myNavHostFragmentName)
 return navController.navigateUp()
} 
```
----
### Adding a Overflow Menu:
OverFlow menu appears on the action bar.

Steps:
1. Add the fragment that you want to navigate to on the navigation graph. 
2. Create a menu. Its located in the **_res folder -> menu_**. Type the name of the menu and resource type of menu. 
3. Go to design tab and drag and drop the menu item from palette. 
4. Add the id in the attribute same id as the fragment you want to navigate to, this will tell navigation which destination we want to navigate to. So it’s important to match the ID exactly. If the id does not match its destination the menu will not navigate. 
5. Now select the fragment/activity you want to add the menu to. First, we need to tell Android that we’re going to have a menu associated with our selected fragment/activity. By adding following code in **_onCreateView()_** method:
```
setHasOptionsMenu(true)
```
6. Menu are created in a method called **_onCreateOptionsMenu_**, which you override.
```
override fun onCreateOptionsMenu(menu: Menu?, inflater: MenuInflater?){
  super.onCreateOptionsMenu(menu, inflater)
  inflater?.inflate(R.menu.name_of_menu, menu)
}
```
7. When menu item is selected, the fragments on options items selected method will be Called.
```
override fun onOptionsItemSelected(item: MenuItem?): Boolean { 
  return NavigationUI.onNavDestinationSelected(item!!, view!!.findNavController()) || super.onOptionsItemSelected(item)
}
```
We did not have to create an action connecting one fragment to another fragment inorder to navigate. Menus are often used to navigate from more than one destination, and there is no way to specify different menu actions for each destination to navigate from. So when we use menus, we usually navigate directly to destination rather than use actions. 

----
### Adding Safe Arugments

Fragments contain Arugments in the form of an Android bundle, which is a key value store. A key value store, also know as a dictionary or associate array, is a data structure we use a unique key such as string to fetch an associated value. There are limited types that can be stored as value in a bundle. Primitive types such as char, int and float, along with various types of arrays, char sequence and a few data classes such as array list.  

To pass parameteres from fragment A to fragment B, you create an instance of the argument bundle class in fragment A. Then set your argument data in the bundle by pushing in associated key string in value pairs. Finally, you set the data into the Arugments. 

```
Val argBundle = Bundle()
argBundle.putString(NAME_KEY_STRING, “content”)
argBundle.putInt(SERIAL_KEY_INT, 42)

Val fragment = FragmentB()
fragment.arguments = argBundle
```
There are few ways in which the above code might generate bugs. Types. It’s up to the developer to make sure that the types of arguments match. There is no way to guarantee that what you pass into fragment A, is what the recipient asks for on the other end. 

Fortunately, navigation includes a feature called Safe Args that can help. Safe Args is a Gradle plugin that generates code to help guarantee that the arguments on both sides match up, while also simplifying argument passing. In order to use Safe args:
Steps:
1. First add the navigation Safe Args Gradle plugin dependency into the project Gradle file. 
```
dependencies {
   …
  "android.arch.navigation:navigation-safe-args-gradle-plugin:$version_navigation"

   // NOTE: Do not place your application dependencies here; they belong
   // in the individual module build.gradle files
}
```
2. At the top of the app gradle file after all of the other plugins, you can add the apply plugin statement with the AndroidX navigation safe args plugin, and now you can start using safe arguments.
```
// Adding the apply plugin statement for safeargs
apply plugin: 'kotlin-kapt'
apply plugin: 'androidx.navigation.safeargs'
```
3. Do a clean built. 
4. Now use the actions, generated with safe args dependency within the fragment. And add the arguments required to pass as parameters. You can add the arguments in the Nav Graph of the fragment you want to pass the data to.  
```
view.findNavController().navigate(FragmentDirections.actionFragToAnotherFrag(parm1, parm2))
```
Within the fragment you want to get the data in we use the generated fragment args class to get the arguments from the argument bundle.
```
val args = FragmentArgs.fromBundle(arguments)
```
Note after using Safe Args you can use nav directions. It’s the self generated action object that is created starting with the name of the **_Fragment_Name_Direction.action._**.

----
### Intents and sharing
An intent indicates intention of your app. It’s a description of something that the app wants an activity to perform.
There are two types of intent:
1.Explicit  - An explicit intent is used to launch an activity using the name of the target activity class, and they are typically only used to launch other activities within your application.
The navigation component does this for you when you navigate to other activities in the navigation graph.

2.Implicit: Implicit intent provide an abstract description of the operation to be performed, and they most often are used to launch activities that are exposed by other applications. 

When multiple Android apps can handle the same implicit intent, Android will pop up a chooser with the list of compatible apps so that the user can select the desired on to handle the request. 

Implicit intents are important because they allow an app to request something from another app without having to know anything about that other app. 
Each implicit intent must have an action. These actions are completely different than the actions that we have in the navigation graph. Actions are used in intents to describe the type of thing that is to be done. Common actions are defined in the intent class such as view, edit, or dial. 

In addition with action implicit intents have a category and datatype to further describe the operation. Categories aren’t always used,  but are used to further disambiguate the action. An example of how categories are used is in the main entry point action. The categories used along with the main entry point to launch an available music player or a mapping application. 

Finally, implicit intents can include a datatype such as text or a JPEG image, which allows application to be chosen based on the data types they can accept.
All activities must be registered in the Android manifest to be launched. Activities that are only launched explicitly can be declared with just an activity tag, while implicitly launched activities require an IntentFilter. 

An IntentFilter is used to exposed that your activity can respond to an implicit intent with a certain action, category, or type. 
For android launcher to start an activity the activity needs to be registered in the Android manifest with the correct IntentFilter. 
We can see what this correct IntentFilter looks like in the MainActivity of our app. The IntentFilter registers our MainActivity with Android as an entry point for out application with the MAIN action, designed to be used by the application launcher because of the LAUNCHER category. If an activity in the app didn’t have this IntentFilter, the app wouldn’t appear in the launcher. 

#### How To user Implicit Intent:
Steps:
1. Create button on the action bar like before using the **_setHasOptionMenu(true)_** and overriding the **_onCreateOptionsMenu()_**.

2. Create share intent method using the type of action and type of data to share. Which will show list of all the app that will work for this implicit intent to work. Then we can add what is known as the intent extra. Intent extra are a key value data structure similar to what we use in the bundle for fragments arguments. They’re used to provide arguments for the intent. Some arguments types such as text have predefined keys. Since we are going to share a string, we’ll add the string with the predefined intent **_EXTRA_TEXT_** key.
```
val shareIntent = Intent(Intent.ACTION_SEND)
shareIntent.setType(“text/plain”).putExtra(Intent.EXTRA_TEXT, getString(R.string.share_success_text, args.numCorrect, args.numQuestions))
```

3. Override the **_onOptionItemSelected()_** method and add logic to tell the app what needs to be done when the option is selected.
Since text sharing intent is so common there is also a easy way to use it. By using the ShareCompat. 
```
private fun getShareIntent(): Intent {
  var args = FragmentArgs.fromBundle(arguments)
  return ShareCompat.IntentBuilder.from(activity).setText(getString(R.string.something).setType(“text/plain”).intent
}
```
4. We also need to account for situation where there are no apps that can perform the intended action. In that case we need to handle this exception otherwise our app will crash. 

5. In our case we will hide the share menu option if we can’t share. In **_onCreateOptionsMenu()_** method we can check to see if the intent will resolve to an activity. We can use the package manager to know about every activity that is registered in the AndroidManifest across every application, so it can be used to see if our implicit intent will resolve to something.
```
if (null == getShareIntent().resolveActivity(activity!!.packageManager)){
  menu?.findItem(R.id.share)?.setVisible(false)
  }
```
----

### Adding the Navigation drawer
The navigation drawer is a panel that slides out from the edge of the screen, and it typically contains header and a menu. It’s hidden when not in use on phone sized devices, and appears when the user swipes a finger from the starting edge of the screen, or when at the starting destination of the app, the user touches the drawer icon, also known as the hamburger icon in the action bar. 

The navigation drawer is part of the material library, a library used to implement patterns that are part of Google’s material design language. 

Steps:
1. Include its dependency into app gradle file. 
```com.google.android.material:material:$supportlibVersion”```

2. Add the fragment you want to add on the drawer in the navigation graph. 

3. The navigation drawer is based upon a menu resource. So we need to create a new nav drawer menu. And add menu items along with id, title, and icon to display. Add as many item as needed.

4. Go to **_mainActivity_** layout and implement the drawer layout as the outer container of the main layout in the activity. 
``` <androidx.drawerlayout.widget.DrawerLayout
android:id=“@+id/drawerLayout”
android:layout_width=“match_parent”
android:layout_height=“match_parent”>
```

5. Just before the end of our drawer layout, we add the navigation view from the material library, which gives our navigation drawer the material look and feel. The tag contains an attribute that points to the nav drawer menu we just created.
```<com.google.android.material.navigation.NavigationView
android:id=“@+id/navView”
android:layout_width=“wrap_content”
android:layout_height=“match_parent”
android:layout_gravity=“start”
app:menu=“@menu/navdrawer_menu”/>
```
6. On the **_MainActivity_** file declare a lateinit drawer layout variable, and initialize it from the activity main binding.
```
lateinit var drawerLayout: DrawerLayout
drawerLayout = binding.drawerLayout
```

7.  When we set up the action bar with the navController, We now need to include the drawer layout as the third parameter to the method. 
```NavigationUI.setupActionBarWithNavController(this, navController, drawerLayout)```
We also need to setup the NavigationUI to know about the navigation view.
```NavigationUI.setupWithNavController(binding.navView, navController)```
and in our onSupportNavigateUp activity method, we need to use **_NavigationUi.navigateUp_** with the drawer layout as a parameter instead of navController navigateUp. 
```
override fun onSupportNavigateUp(): Boolean { 
  val navController = this.findNavController(R.id.myNavHostFragment)
  return NavigationUI.navigateUp(drawerLayout, navController)
}
```
This is so the navigation UI can replace the Up button with the navigation drawer button when we get to the start destination. 
  
9. To create a header for the Navigation drawer we just need to create an xml layout and add  it in the NavigationView tag with ```app:headerLayout=@layout/nav_header``` tag.
----
### Navigation Listeners
For the navigation drawer, you can swipe from the starting side and the drawer will appear. Now the button is only visible at the start destination, but the swipe works anywhere within the app. You might want to prevent the drawer from working in certain places. We can will be using navigation listener to demonstrate this. 

Navigation listeners are an interface that contains a single method that gets called every time we navigate. They allow us to react and do something during navigation, or in our case, block the drawer from coming out after navigation away from the start destination.
Steps:
1. Create a navigation listener within onCreate, it gets called whenever the destination changes. We get back the destination and we could use this for all custom functionality.
```
navController.addOnDestinationChangedListener { nc: NavController, nd: NavDestination, args: Bundle? ->
   if (nd.id == nc.graph.startDestination) {
       drawerLayout.setDrawerLockMode(DrawerLayout.LOCK_MODE_UNLOCKED)
   } else {
       drawerLayout.setDrawerLockMode(DrawerLayout.LOCK_MODE_LOCKED_CLOSED)
   }
}
```
