---
title: Katana
date: 2016-01-10 14:54:18
---

Katana is an open source Continuous Integration server based on the BuildBot project. It is used at Unity Technologies by our 200+ developers daily and currently supports over 500 build configurations and multiple thousands of builds per day. Although Katana is based on the BuildBot project a lot of the features built for Katana have been influenced by Unity previous experience with TeamCity, where the front-end has a far greater focus. The reason for this is that at Unity we follow more of a frequent integration due to the complexity of our build processes (each push from a development branch to trunk can take multiple hours with hundreds of builds), which requires much more input and understanding from our developers with the build process. This required us to implement features that help to make the experience with our CI software better for all our developers.

Although Katana is not my main responsibility at Unity I have a big influence on how it's being developed going forward. The project can be found on [GitHub](https://github.com/Unity-Technologies/katana) and is maintained by a number of employees at Unity. 


## My contributions to the project

During my time on the project I've helped out on a number of different areas of the project.

- Adding real-time data to the front-end using Autobahn
- Implementing features such as:
	- Comparing build results across branches
	- Adding tags and filtering to the builders page
	- Building on specific and all slaves from the force build form
	- Updating the JSON API to allow for support with other software used at Unity
- Maintaining the front-end
- Implementing and maintaining custom builders for the many builds we require at Unity