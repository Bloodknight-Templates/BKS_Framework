# Project Readme


Designed to be a quick and easy startup project for just getting down to writing the code from tutorials.

## Libraries
- GLFW [https://github.com/glfw/glfw]
- glad [https://github.com/Dav1dde/glad]
- stb_image (may add more later) [https://github.com/nothings/stb]

### Testing
In the `testing` subfolder there exists a few files whos contents can be copied to replace the main.cpp file, these are there so that eac part of the library set up can be tested.

#### Notes
This project is intended eventually to be fully cross platform, this is why cmake is being used. I am aware that the cmake stuff is very ***Windows*** at the moment, this is because this is the main platform I am stuck with for the moment.

###### GLFW
- ***GLFW is supplied for VS2022***, until i learn how to integrate a fully external library replacing the libs is a manual process, instructions for building glfw can be found on the glfw website.

###### GLAD
- The decision was made for the moment to "kitchen sink" this entire thing, The generator was used to select GL 4.6 and added all extensions, this is mostly unnecessary but, replacing the glad files is fairly trivial if you need a different setup.




