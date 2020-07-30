# Flightmap-Navigation-Sdk-Android
The FlightMap Navigation SDK for Android gives you all the tools that you need to add turn-by-turn navigation to your apps. 
Get up and running in a few minutes with our drop-in turn-by-turn navigation, or build a more custom navigation experience with our navigation UI components.

# Integrating the Navigation SDK into your project
Before developing your app with the FlightMap Navigation SDK, you'll need to add several dependencies. Android Studio uses a toolkit called Gradle to compile resources and source code into an APK. 
The build.gradle file is used to configure the build and list dependencies. You should add the Navigation UI SDK as a dependency in the repositories section.
Declaring this dependency will automatically pull in both the FlightMap Navigation SDK for Android and the FlightMap Maps SDK for Android. This is why the core Navigation 
SDK and Maps SDK dependency lines are not listed in the installation code snippet below.

- In the module-level build.gradle (root project) file, add the following dependencies in your your build.gradle (root) contents

```bash
allprojects {
    repositories {
        google()
        jcenter()
        maven { url 'https://mapbox.bintray.com/mapbox' }
        maven { url 'https://dl.bintray.com/flightmap/com.flightmap' }
        maven { url 'https://dl.bintray.com/flightmap/flightmapnavigationsdk' }
        maven { url 'https://dl.bintray.com/flightmap/flightmapjavasdk' }
    }
}
```


- In the app module -level build.gradle (app module) add the following dependencies
before implementing  the library Please Add :- 
```bash
android {
 compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}
// dependency for FLightMap Maps Sdk
  implementation 'flightmapsdk.flightmaplightsdk:flightmap: 1.4.0'
// dependency for FLightmap Navigation Sdk
  implementation 'flightmapnavigationsdk:mapbox-android-navigation-ui:1.5.2'
  ```
# Manifest.xml

-<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />

# To create NavigationRouteOptions:
    private void getRoute(Point origin, Point destination) {
        NavigationRoute.builder(this)
                .accessToken(Add Your Fm Token Here)
                .origin(origin)
                .destination(destination)
                .build()
                .getRoute(new Callback<DirectionsResponse>() {
                    @Override
                    public void onResponse(Call<DirectionsResponse> call, Response<DirectionsResponse> response) {

                        Log.d(TAG, "Response code: " + response.code());
                        if (response.body() == null) {
                            Log.e(TAG, "No routes found, make sure you set the right user and access token.");
                            return;
                        } else if (response.body().routes().size() < 1) {
                            Log.e(TAG, "No routes found");
                            return;
                        }

                        currentRoute = response.body().routes().get(0);


                        if (navigationMapRoute != null) {
                            navigationMapRoute.removeRoute();
                        } else {
                            navigationMapRoute = new NavigationMapRoute(null, mapView, mapboxMap, R.style.NavigationMapRoute);
                        }
                        navigationMapRoute.addRoute(currentRoute);
                    }

                    @Override
                    public void onFailure(Call<DirectionsResponse> call, Throwable throwable) {
                        Log.e(TAG, "Error: " + throwable.getMessage());
                    }
                });
    }
    
    
  - <img src="https://github.com/Jungle-Works/Flightmap-Navigation-demo-Android/blob/master/destinatonroute.gif" width="300" />



# Start Navigation Route :
      button.setOnClickListener(new View.OnClickListener() {
                    @Override
                    public void onClick(View v) {
                        //set this true to automaticaly navigate the map
                        boolean simulateRoute = false;
                        NavigationLauncherOptions options = NavigationLauncherOptions.builder()
                                .directionsRoute(currentRoute)
                                .shouldSimulateRoute(simulateRoute)
                                .build();

                        NavigationLauncher.startNavigation(MainActivity.this, options);
                    }
                });
               
               
  -  <img src="https://github.com/Jungle-Works/Flightmap-Navigation-demo-Android/blob/master/startnavigation.gif" width="300" />
