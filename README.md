# hello-world

We need communication ports between an external cluster and internal Informatica server to be predictable because there's a strict firewall in place. So we need some way to make sure only a known set of ports is used for communication between client and cluster.

Range of ports that the MapReduce AM can use when binding. Leave blank if you want all possible ports. For example 50000-50050,50100-50200

The AM's IPC port is indeed used directly by clients and are controllable on the serving AM via the yarn.app.mapreduce.am.job.client.port-range config. It still has to be a range though, and the range must be chosen by keeping in mind that it will also effectively limit the number of AMs you can run on the host.

The AM's web port is also served on an ephemeral port, but this is a non-concern cause clients do not access the AM web port directly; they go via the RM's proxy service (wherein the RM makes the GET HTTP requests to the actual AM port, within the cluster).

Does yarn.app.mapreduce.am.job.client.port-range not solve your need? There's no IPC proxying today to eliminate the range requirement, unfortunately. The con of not having the IPC port ranges open is not too fatal, as the job can still get a completed notification once it gets moved to the job history server (and the RM redirects the client to it)
