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
to set the layout in Activity. But setContentView does not work for Fragment. Instead we will be using 
```
DataBindingUtil.inflate<FragmentTitleBinding>(inflater,R.layout.fragment_title,container,false)
return binding root
```

### Navigation Component
### Creating different menu
### Using implicit intent
