
Today’s progress focused on revising network namespaces, reinforcing how they provide isolated network stacks (interfaces, routes, IP tables) for containers or processes. Reviewed key commands like ip netns add, ip link set, and connecting namespaces using veth pairs.

Also revisited the Kubernetes perspective, linking how pods use Linux network namespaces for isolation and how CNI plugins configure them automatically during pod creation.
