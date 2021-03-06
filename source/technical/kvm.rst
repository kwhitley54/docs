.. _kvm:

==============================
KVM
==============================

____________
Introduction
____________

OpenStack is an Infrastructure as a Service (IaaS) platform that allows you to create and manage virtual environments. Chameleon provides an installation of OpenStack version 2015.1 (Kilo) using the KVM virtualization technology at the `KVM@TACC <https://openstack.tacc.chameleoncloud.org>`_ site. Since the KVM hypervisor is used on this cloud, any virtual machines you upload must be compatible with KVM.

This documentation provide basic information about how to use the OpenStack web interface and provides some information specific to using OpenStack KVM on Chameleon. The interface is similar to the bare metal sites `CHI@TACC <https://chi.tacc.chameleoncloud.org>`_ and `CHI@UC <https://chi.uc.chameleoncloud.org>`_. However, the resources that you are using are virtual, rather than being tied to physical nodes. Familiarity with some concepts, such as :ref:`gui-key-pairs` are also required for KVM.

___________________________
Work with KVM using the GUI
___________________________

An easy way to use OpenStack KVM on Chameleon is via the GUI, which is similar to the GUIs for `CHI@TACC <https://chi.tacc.chameleoncloud.org>`_ and `CHI@UC <https://chi.uc.chameleoncloud.org>`_. You log into the web interface using your Chameleon username and password. 

.. note::
   If you change your Chameleon password in the portal, the change will propagate to the OpenStack KVM interface in about 5 minutes.

After a successful log in, you will see the *Overview* page as shown below. This page provides a summary of your current and recent usage and provides links to various other pages. Most of the tasks you will perform are done via the menu on the lower left and will be described below. One thing to note is that on the left, your current project is displayed. If you have multiple Chameleon projects, you can change which of them is your current project. All of the information displayed and actions that you take apply to your current project. So in the screen shot below, the quota and usage apply to the current project you have selected and no information about your other projects is shown.

.. figure:: kvm/overview.png

Managing Virtual Machine Instances
__________________________________

One of the main activities you’ll be performing in the GUI is management of virtual machines, or instances. Go to *Project* > *Compute* > *Instances* in the navigation sidebar. For instances that you have running, you can click on the name of the instance to get more information about it and to access the VNC interface to the console. The dropdown menu to the left of the instance lets you perform a variety of tasks such as suspending, terminating, or rebooting the instance.

.. figure:: kvm/instances.png

Launching Instances
___________________

To launch an *Instance*, click the *Launch Instance* button. This will open the *Launch Instance* dialog.

.. figure:: kvm/launchdetails.png

Follow these steps to configure *Details* tab:

#. Provide a name for this instance (to help you identify instances that you are running)
#. Choose a *Flavor* for the Instance. Flavors refer to the virtual machine's assigned memory and and disk size. Different images and snapshots may require a larger Flavor. For example, the ``CC-CentOS7`` image requires at least an ``m1.small`` flavor.
   
   .. tip:: If you select different flavors from the Flavor dropdown, their characteristics are displayed on the right.

#. Select the amount of resources (Flavor) to allocate to the instance.
#. Select the *Instance Boot Source* of the instance, which is either an *Image*, a *Snapshot* (an image created from a running virtual machine), or a *Volume* (a persistent virtual disk that can be attached to a virtual machine). If you select *Boot from image*, the *Image Name* dropdown presents a list of virtual machine images that we have provided, that other Chameleon users have uploaded and made public, or images that you have uploaded for yourself. If you select *Boot from snapshot*, the *Instance Snapshot* dropdown presents a list of virtual machine images that you have created from your running virtual machines.

When you are finished with this step, go to the *Access and Security* Tab.

.. figure:: kvm/launchaccess.png

#. Select an SSH keypair that will be inserted into your virtual machine. You will need to select a keypair here to be able to access an instance created from one of the public images Chameleon provides. These images are not configured with a default root password and you will not be able to log in to them without configuring an SSH key.
#. If you have previously defined *Security Groups*, you may select them here. Alternatively, you can configure them later.

Set up network using *Network* tab.

.. figure:: kvm/launchnetwork.png

#. Select which network should be associated with the instance. Click the ``+`` next to your project’s private network (PROJECT_NAME-net), not ``ext-net``.

Now you can launch your instance by clicking on the *Launch* button and the *Instances* page will show progress as it starts.

.. _kvm-associate-ip:

Associating a Floating IP Address
_________________________________

You may assign a Floating IP Address to your Instance by selecting *Associate Floating IP* in the dropdown menu next to your Instance on the *Instances* page.

.. figure:: kvm/associatemenu.png

This process is similar to :ref:`baremetal-gui-associate-ip` on `CHI@TACC <https://chi.tacc.chameleoncloud.org>`_ and `CHI@UC <https://chi.uc.chameleoncloud.org>`_ bare metal sites.

Key Pairs
_________

You will need to import or create SSH :ref:`gui-key-pairs`. This process is similar to the process performed on `CHI@TACC <https://chi.tacc.chameleoncloud.org>`_ and `CHI@UC <https://chi.uc.chameleoncloud.org>`_ bare metal sites.

Security Groups
_______________

*Security Groups* allow you to specify what inbound and outbound traffic is allowed or blocked to Instances. Unlike the `CHI@TACC <https://chi.tacc.chameleoncloud.org>`_ and `CHI@UC <https://chi.uc.chameleoncloud.org>`_ bare metal sites, `KVM@TACC <https://openstack.tacc.chameleoncloud.org>`_ observes Security Groups for Instances.

.. note:: By default, all inbound traffic is blocked to `KVM@TACC <https://openstack.tacc.chameleoncloud.org>`_ Instances, including SSH. You must apply a Security Group that allows TCP port 22 inbound to access your instance via SSH.

To create a Security Group, click *Projects* > *Compute* > *Access and Security* in the navigation side bar. 

.. figure:: kvm/securitytab.png

Click the *+Create Security Group* button to open the *Create Security Group* page.

.. figure:: kvm/createsecurity.png

Enter a *Name* for your *Security Group*, and optionally provide a *Description*. Then click the *Create Security Group* button. 
Now, you should see your *Security Group* listed on the *Access and Security* page.

.. figure:: kvm/grouplist.png

Click the *Manage Rules* button in the *Action* column to open the *Manage Security Group Rules* page.

.. figure:: kvm/managerules.png

The default Security Group allows outbound IPv4 and IPv6 traffic for *Any IP Protocol* and *Port Range*. If no entry for *Ingress*, no inbound traffic will be allowed. You may add an additional rule by clicking on the *+Add Rule* to open the *Add Rule* dialog.

.. figure:: kvm/addrule.png

In this dialog, you can specify *Custom TCP Rule* (or *Custom UDP Rule* or *Custom ICMP Rule*), a *Direction* (*Ingress* for inbound traffic to your Instance or *Egress* for outbound traffic) and a *Port*. Alternatively, you can use a pre-defined rule in the *Rule* dropdown, such as *SSH*. when you are finished, click *Add*.

.. _kvm-security-group:

Adding a Security Group to an Instance
______________________________________

Once you have defined a *Security Group*, you may apply it to an Instance by clicking *Project* > *Compute* > *Instances* in the navigation sidebar and clicking the *Edit Security Groups* option in the *Actions* dropdown.

.. figure:: kvm/editaction.png

The *Security Groups* tab in the *Edit Instance* dialog will pop up. 

.. figure:: kvm/editinstance.png

You may click the *+* button next to the Security Group you wish to apply in the *All Security Groups* list on the left. Once you are finished, click *Save* to finish the process.

