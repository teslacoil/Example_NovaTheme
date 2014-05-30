#Dynamic Calendar Icon API#
The Dynamic Calendar Icon API allows a calendar app, or icon theme, to specify a set of 31 icons for the launcher to use so the current day of month can be displayed as the app icon.

There are three parts to this. The Launcher, the Calendar App, and the Icon Themes. Nova Launcher 3.0 supports this, however other launcher developers are encouraged to support this API as well. Please do not deviate from it without reason to keep it consitent for other developers.

##Icon Theme Developers##
For icon themes this API makes use of a `<calendar />` tag in the standard `appfilter.xml` with two attributes, `component` (same format as used in the `<item />` tag) and `prefix`. The `prefix` attribute specifies the first part of the name of a series of drawables, for example `prefix="ic_calendar_"` signifies that there are a series of drawables named `ic_calendar_1`, `ic_calendar_2`, `ic_calendar_3`, all the way up to `ic_calendar_31`. If a `calendar` tag is present for a `component`, then any item tag for the same `component` is ignored. This means that icon theme developers can keep the `item` tag in their `appfilter.xml` for use for launchers that do not support this API. For example to theme the Today Calendar app with a set of drawables named `ic_today_1`, `ic_today_2`, etc.

    <calendar component="ComponentInfo{com.underwood.calendar_beta/com.android.calendar.AllInOneActivity}" prefix="ic_today_" />

When done from an icon theme, this does not require any special support from the app. So you may also use this to theme Google's, Samsung's, and any other calendar apps.


##Calendar App Developers##
For calendar app developers, to support dynamic icons without any themes, you add an array to `res/values/arrays.xml` listing the drawables and reference this array in your `AndroidManifest.xml` as `meta-data` for the activity.

For example:

`res/values/arrays.xml`

    <array name="calendarIcons">
        <item>@drawable/calendar1</item>
        <item>@drawable/calendar2</item>
        <item>@drawable/calendar3</item>
        <item>@drawable/calendar4</item>
        <item>@drawable/calendar5</item>
        ...
    </array>

`AndroidManifest.xml`

    ...
    <activity
        android:name=".MainActivity">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
    <meta-data android:name="com.teslacoilsw.launcher.calendarIconArray"
         android:resource="@array/calendarIcons" />
    </activity>
    ...

Though the name is in Nova Launcher's namespace (com.teslacoilsw.launcher), this same name can and should be used by other launchers. There is no need for a launcher to create their own calendarIconArray parameter as long as they are compatible with this API.

##Launcher Developers##
Though this first shipped with Nova Launcher, I encourage other launcher developers to make use of it. It is the launcher's responsibility to track the date change and update the icons appropriately. For calendar apps such as Today that support the API, make sure to respect it when creating iconback based icons from themes, and to update. For icon themes that theme other apps, make sure to track the date even if the calendar app doesn't normally have a dynamic icon.
