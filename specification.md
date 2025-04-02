### Abstract

Deplon is a deployment tool aims to maximize developer experience (DX) and minimize time spent in DevOps.
An Deplon system is composed of one or more worker node and single control node.
Usually worker nodes don't have information about control node and they do not ask control node for data.
So when a deployment is performed, the control node must order desired worker node to update required informations.
This mechanism allows control node to not be on a fixed machine and to be offline when they are not performing any deployment or updates.
Control node not being on a fixed machine makes Deplon works with existing CI/CD tools like GitHub Actions, lowering or even removing operation cost of maintaining always online machine and building deployment images.
