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
-------------------------------------
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

### Creating different menu
### Using implicit intent
