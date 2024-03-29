# Project Title

Trending GitHub Repositories

## Overview of the app

Android app that lists all popular GitHub repositories, lists name, description, stars, owner name and owner avatar related to each repository.

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes.

### Prerequisites

What things you need to have installed
```
Android Studio 3.5 or higher
Android SDK Platform 29
Java JDK 12
```

### Installing

Simply import the project into android studio.

```
Android studio>File>Open
```
Browse to the project folder and select open, then install any missing android sdk platforms and you are good to go.

## Architecture

#### [Android Architecture Component MVVM(Model-View ViewModel)](https://developer.android.com/topic/libraries/architecture)
 Android architecture components are a collection of libraries that help you design robust, testable, and maintainable apps.

## Libraries used and why:

#### [Paging Library](https://developer.android.com/topic/libraries/architecture/paging)
To load small chunks of data a time, rather than load all data at once.

#### [Glide](https://github.com/bumptech/glide)
To retrieve and display the repository owner avatar.

#### [Volley](https://developer.android.com/training/volley)
To make network requests to GitHub API, since the response has load of fields and we only interested at some of 		them(name,description,stars,owner name,owner avatar) its better to use Volley StringRequest then parse the request to 		json object then extract only the fields needed, better than using Retrofit with JsonConverter which forces you to create 	class that contains all fields of the response, means more memory allocated.

#### [Material Design](https://material.io/develop/android/docs/getting-started/)
To use some components such as BottomNavigationView.

#### [ViewModel](https://developer.android.com/topic/libraries/architecture/viewmodel)
Will contains all app background process, retrieve data, loading data, caching 	data, and also handle changes of activity/fragment lifecycle.

#### [Room Persistence Library](https://developer.android.com/topic/libraries/architecture/room)
Part of Android Architecture Component as well, to cache retrieved data from the API

#### [CirculeImageView](https://github.com/hdodenhof/CircleImageView)
To display the repository owner avatar as circle image.

#### [ButterKnife](https://jakewharton.github.io/butterknife/)
To simplify binding and unbinding views.

#### [LiveData](https://developer.android.com/topic/libraries/architecture/livedata)
To continuesly listen data changes in cache database and instantly load it if any changes occurs.

## Features:

* The more you scroll the more data is loaded
* Swipe each item left or right to delete
* Settings page which allower you to:
	* Clear all cached data
	* Change how many items to load per page from the local cache

## Project Structure:

* Model:    *`contains the repository class and its builder`*
	* Repo.java
	* RepoBuilder.java
* Repository:  *`contains classes that will be responsible of retrieving and caching data`*
	* Dao:  *`contains the database access object(dao) which directly query the database`*
		* RepoDao.java
	* Database:  *`database`*
		* RepoDatabase.java 
	* Service:  *`github service, that retrieves data from github API`*
		* GitHubService.java
	* Repository.java
* UI:
	* Activities:  *`home activity that will display the 2 fragments below`*
		* Main.java
	* Fragments:
		* Settings:  *`settings fragment`*
			* SettingsFragment.java
		* TrendingRepositories: *`trending repositories fragment`*
			* TrendingReposFragment.java
	* Recycler:  *`adapter for the recyclerView`*
		* RepoPagedAdapter.java
	* ViewHolder:  *`of each item in the adapter`*
		* RepoHolder.java
* Utiliy:  *`contains static methods and constants that will be used by other classes`*
	* Utility.java
* ViewModel:  *`this class is responsible from watch data and handle activity/fragment lifecycle`*
	* RepoViewModel.java

## How it works:
	
At the start of the app, it checks whether there is cached data in the local room database, if there is some, then it loads it N item per page(This N default value is 30, but can be changed in the settings), if no data is cached in the database then, it checks if there is an active internet connection to retrieve data from the GitHub API, if so then it makes network requests using Volley to get data, after each request the page number that is passed to the API is inceremented, then each request retrieves 30 item per page, each request is delayed by 1000 ms(1 second) to avoid getting blocked by the API, the data received is immediately inserted into the database, since we have liveData watching and listening on any changes on the database, it immediately loads it as PagedList into PagedListAdapter which handle displaying data in RecyclerView.

## License

This project is free to use.
