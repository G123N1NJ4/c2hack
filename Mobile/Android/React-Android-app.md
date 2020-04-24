# How to pentest React app

React Native is a mobile application framework that is most commonly used to develop applications for Android and iOS by enabling the use of React and native platform capabilities. 

## Dynamic debug with the map file
On the Android application, you can unzip it and browse the **assets** folder. It should contain:
- index.android.bunle;
- index.android.bundle.map (can be present, map files contain the source mapping in order to map minified identifiers).

Then you can analyse dynamically the app by creating a file: `index.html` with:
```
<script src="index.android.bundle"></script>
```

Load it on Google Chrome and then use the developer tools.

## Links
https://blog.assetnote.io/bug-bounty/2020/02/01/expanding-attack-surface-react-native/

