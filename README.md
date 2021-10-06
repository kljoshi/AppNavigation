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

### Navigation
#### Navigate:
We can navigate between different activities and fragments and activity. The previous activities are arranged in stack which is known as back stack. System back key will pop the top of the stack. Fragment Back Stack works the same, the fragments are arranged in stack as well.

#### Navigation Component:
The navigation component simplifies many navigation tasks. More importantly it helps developer to follow principle of navigation.
##### First principle: There’s Always a Starting Place (This screen should also be the last screen that the user sees after pressing the back button.)
##### Second principle: You Can Always Go Back (The navigation state of your app should be represented with a last in first out structure.)
##### Third principle: Up button goes back (mostly) If the user is at the first screen the user should not be shown the up button. 
### Creating different menu
### Using implicit intent
