---
title: Vulkan Notes
layout: post
---



### Resources in Vulkan

1. Buffer - raw arrays of bytes.
2. Images - multidimensional data with associated metadata.

#### Buffers

Three important data structure related to creating a buffer

1. **VkBuffer** - creates a buffer, but doesn't yet allocate the memory for it.
2. **VkDeviceMemory** - located on device. Though this memory has to be allocated (vkAllocateMemory) and freed (vkFreeMemory) explicitly, the handle to buffer is the first data structure (VkBufferMemory) and that is which is used for binding buffers. (Question: Where else do we explicitly refer to VkDeviceMemory ?)
3. **void* mappedMemory** - located on host. For copying any data to VkDeviceMemory just copy the data to this mapped memory on host.


### Images

 ---- To be covered ----



### Resource Descriptors

Shaders need to access the resources loaded by the program. For vertices and its attributes we use ```vkCmdBindVertexBuffers``` and ```vkCmdBindIndexBuffer``` to bind them and access them using the 'in' keyword in the shader.

```
layout(location = 0) in vec2 inPosition;
layout(location = 1) in vec3 inColor;
```

However for other resources like model view transformations (stored as VkBuffer) , texture images, depth images (stored as VkImage), we define a resouce descriptor explicitly and bind them using ```vkCmdBindDescriptorSets```





## Some other Random Notes:

1. Seems like we are not able to control the line width in MacOS. This is relevant when the topology is one of the "line" options.
   Check this : https://community.khronos.org/t/macos-vulkan-at-runtime-vkcreatedevice-fails-when-vkphysicaldevicefeatures-widelines-vk-ture-and-doestnt-support-vkcmdsetlinewidth-api-as-well/106443/2
   Also, check for VkPhysicalDeviceFeatures::wideLines - This was not enabled on macos for me.