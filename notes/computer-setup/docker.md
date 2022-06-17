# Docker

## What is Docker

Docker is a tool that helps you build, run and share "containerized" apps. Being "containerized" means that each app cannot access the memory/disk/network that is accessed by other processes until explicitly allowed so. Additionally, since each container is often dedicated to just one app, it can be built relatively easily by following a few predefined steps, and the result will be identical regardless of which machine you run these steps on. As we will soon see, these properties make docker very useful for recreating "programming environments".

## Why tho?

A helpful way to understand why containers can be useful is to compare them to virtual machines (VM):

![Container vs. VM](https://www.cloudsavvyit.com/p/uploads/2019/06/bc4f8762.png?trim=1,1&bg-color=000&pad=1,1)

The key difference in the diagram above is the absence of multiple copies of "guest OS" in the containerized set up. Essentially, the motivation for using containers is to achieve the **isolation** and **reproducibility** enjoyed by VMs without suffering from the high resource overhead of VMs.

**Isolation** means that each containerized app runs independently without interfering with others. This is especially useful when dealing with dependency conflicts. For example, one package might require `package_A=3.1.5` while another requires `package_a>=4.0`, making it impossible to run both apps if they are directly running on our machine. When each app is running containerized, each container will include a copy of its dependencies with the versions specific to that app. Since different containers can not "see" each other, there are no conflicts.

**Reproducibility** means that it is easy to set up the same "programming environment" on different machines. Most programs today require dependencies—external libraries or packages that the program is built upon—to function properly. This reduces repetitive work and improves productivity (imagine that every time we want to write a new C program, we have to implement a C compiler from scratch!), but also means that sometimes it is not as simple as "download and play", as we need to install all the required dependencies before a program will run. To compound this issue, not all libraries/packages are available on every major OS, and even when they are there are often differences in how to set them up for each platform. On the other hand, containers only have very basic requirements about the underlying system that are easy to meet. Once these requirements are met, the process of setting up a particular container is identical on any machine, meaning that it can be entirely automated.

### How does Docker work

Although the comparison between docker and VM is helpful for understanding its benefits, the way how Docker works is actually quite different from how VMs work. Instead of a VM, Docker can be considered as a **process configuration manager** that decides what files a can process read/write, which (other) processes it can see, and what networks it can communicate on. The process is still running on your normal OS (called the "host" OS), but with limited interaction with everything else. Most docker containers run on top of Linux kernels. On Windows and Mac systems, there's VM running a miniature Linux to support all the containers. (Check out [this great answer](https://stackoverflow.com/a/56606244/9362448) on StackOverflow)
