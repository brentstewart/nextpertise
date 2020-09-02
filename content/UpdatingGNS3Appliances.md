---
title: "Updating a GNS3 Appliance File"
date: 2020-09-02T08:41:13-04:00
draft: false
author: "Brent"
github: "https://github.com/brentstewart/gns3-registry"
youtube: ""
refs: ["https://securityonion.net/","https://github.com/GNS3/gns3-registry", "https://docs.github.com/en/github/collaborating-with-issues-and-pull-requests/creating-a-pull-request"]
tags: ["GNS3","Git"]
---
This is a long post, but most of it is file contents.  Keep reading!

GNS3 appliance files are descriptions of virtual machines used in network simulations.  The appliance files have suffixes of _.gns3a_ and are included with the GNS3 download.  You can update the files and create new ones.  The goal of this article is to walk through the process of working with appliance files and contributing them back to the community.

On a personal note, submitting a new GNS3 appliance was the first time I contributed to an open-source project.  I'm still learning, but a few years ago I knew _nothing_.  Jeremy Grossman, with GNS3, was patient and helped me understand the process of using Git.  Contributing - even in this minor way - was a real high for me and I'd love for you to be able to share that feeling and contribute to this and other projects.  GNS3a was my "gateway drug" into being a contributor and not just a consumer of open source.

One of the files I've contributed is the Security Onion appliance.  Security Onion is a Linux distribution that focuses on security tools.  Below is the current version (9/1/20) of the GNS3A file.  Before we create a new appliance, let's update this one.

> {  
    "name": "Security Onion",  
    "category": "guest",  
    "description": "Security Onion is a Linux distro for intrusion detection, network security monitoring, and log management. It’s based on Ubuntu and contains Snort, Suricata, Bro, OSSEC, Sguil, Squert, ELSA, Xplico, NetworkMiner, and many other security tools. The easy-to-use Setup wizard allows you to build an army of distributed sensors for your enterprise in minutes!",  
    "vendor_name": "Security Onion Solutions, LLC",  
    "vendor_url": "https://securityonion.net/",  
    "documentation_url": "https://github.com/Security-Onion-Solutions/security-onion/wiki",  
    "product_name": "Security Onion",  
    "product_url": "https://securityonion.net/",  
    "registry_version": 3,  
    "status": "stable",  
    "maintainer": "Brent Stewart",  
    "maintainer_email": "X@X",  
    "usage": "Your default account will have sudo priviledges.  Squil and Squert username and password are configured in the Setup wizard.  MySQL root is set to null.  For more info see https://github.com/Security-Onion-Solutions/security-onion/wiki/Passwords.",  
    "symbol": "securityonion-logo.png",  
    "qemu":  
    {  
        "adapter_type": "e1000",  
        "adapters": 2,  
        "ram": 3072,  
        "arch": "x86_64",  
        "console_type": "vnc",  
        "kvm": "allow"  
    },  
    "images":  
    [  
        {  
            "filename": "securityonion-16.04.6.1.iso",  
            "version": "16.04.6.1",  
            "md5sum": "ca835cef92c2c0daafa16e789c343d1d",  
            "filesize": 2020605952,   
            "download_url": "https://github.com/Security-Onion-Solutions/security-onion/releases/",  
            "direct_download_url": "https://github.com/Security-Onion-Solutions/security-onion/releases/download/v16.04.5.3_20181010/securityonion-16.04.6.1.iso"  
        },  
        {  
            "filename": "securityonion-16.04.5.3.iso",  
            "version": "16.04.5.3",  
            "md5sum": "886b369548c9c3841bc820cc3ab02bd9",  
            "filesize": 1895825408,  
            "download_url": "https://github.com/Security-Onion-Solutions/security-onion/releases/",
            "direct_download_url": "https://github.com/Security-Onion-Solutions/security-onion/releases/download/v16.04.5.3_20181010/securityonion-16.04.5.3.iso"  
        },  
        {  
            "filename": "securityonion-14.04.5.4.iso",  
            "version": "14.04.5.4",  
            "md5sum": "9c7cab756b675beb10de4274a3ad3bc6",  
            "filesize": 1874853888,  
            "download_url": "https://github.com/Security-Onion-Solutions/security-onion/releases/",  
            "direct_download_url": "https://github.com/Security-Onion-Solutions/security-onion/releases/download/v14.04.5.4_20171031/securityonion-14.04.5.4.iso"  
        },
        {
            "filename": "securityonion-14.04.5.3.iso",  
            "version": "14.04.5.3",  
            "md5sum": "fb80ccb2d3c0f3f511823fa5858f87d1",  
            "filesize": 1889533952,  
            "download_url": "https://github.com/Security-Onion-Solutions/security-onion/releases/",
            "direct_download_url": "https://github.com/Security-Onion-Solutions/security-onion/releases/download/v14.04.5.4_20171031/securityonion-14.04.5.3.iso"  
        },
        {
            "filename": "empty30G.qcow2",  
            "version": "1.0",  
            "md5sum": "3411a599e822f2ac6be560a26405821a",  
            "filesize": 197120,  
            "download_url": "https://sourceforge.net/projects/gns-3/files/Empty%20Qemu%30disk/",  
            "direct_download_url": "https://sourceforge.net/projects/gns-3/files/Empty%20Qemu%20disk/empty30G.qcow2/download"  
        }  
    ],  
    "versions":   
    [  
        {  
            "name": "16.04.6.1",  
            "images":   
            {  
                "hda_disk_image": "empty30G.qcow2",  
                "cdrom_image": "securityonion-16.04.6.1.iso"  
            }  
        },  
        {  
            "name": "16.04.5.3",  
            "images":  
            {  
                "hda_disk_image": "empty30G.qcow2",  
                "cdrom_image": "securityonion-16.04.5.3.iso"  
            }  
        },  
        {  
            "name": "14.04.5.4",  
            "images":  
            {  
                "hda_disk_image": "empty30G.qcow2",  
                "cdrom_image": "securityonion-14.04.5.4.iso"  
            }  
        },  
        {  
            "name": "14.04.5.3",  
            "images": {  
                "hda_disk_image": "empty30G.qcow2",  
                "cdrom_image": "securityonion-14.04.5.3.iso"  
            }  
        }    
]  
}  

Most of this is pretty straight forward.  The structure looks like:

A descriptive section
* Name, Description, usage, vendor and product information can be taken from the source website
* Category can be guest, router, firewall, or multilayer_switch.
* Maintainer is the creator.  Notice I've replaced my email for publication to the web.
* Symbol should be a SVG file with a maximum height of 70px.  Either reference an existing symbol or add a new one.

Next is the Qemu section that describes how the VM environment should be constructed.  This is straightforward as well.  Console types are VNC or telnet.  You may have to try different ethernet adapters to see what works, but I recommend starting with the Intel e1000 because this model is supported by most VMs.  Using a para-virtualized adapter may give better performance, so you may also want to try vmxnet3.  Most architectures will be 64bit and RAM requirements will usually be on the website.

That leaves two sections - Images and Versions.  There should be a matching entry in both places.  The images section is a list of virtual hard drives and CD-ROM images to use in the VM and includes:
* Filename, version, and download URL
* md5sum - most Linux distributions include the command __md5sum__.  Download the appliance ISO and use __md5sum myfile.iso__ to generate a checksum.  Most Linux distributions include the checksum on their download page as well, so double check your version.
* filesize is reported on Linux using __ls -l__
* If an empty drive image is needed, GNS3 provides them in different sizes on Github (as referenced above)
The versions section needs to have a name that matches the version number provided in the images section.  That ties the images to the correct version.
Notice that I've set this up to boot to an empty machine and prompt the user to do the installation.  I could also supply a QCOW2 file with Security Onion pre-installed.

Let's update this file.  There are a lot of old images listed as options.  I'll remove the image and version sections for 14.04.5.3 and add the most recent (16.04.7.1).  That will leave users with the last 14.x and two images in 16.x including the latest.  Whether dealing with a distribution or a commercial image, changes made between versions may introduce new processes or bugs so leaving some older images gives users an easy workaround.  Here's the updated file.  Scroll below the output for a discussion of submitting this back to the project.

> {  
    "name": "Security Onion",  
    "category": "guest",  
    "description": "Security Onion is a Linux distro for intrusion detection, network security monitoring, and log management. It’s based on Ubuntu and contains Snort, Suricata, Bro, OSSEC, Sguil, Squert, ELSA, Xplico, NetworkMiner, and many other security tools. The easy-to-use Setup wizard allows you to build an army of distributed sensors for your enterprise in minutes!",  
    "vendor_name": "Security Onion Solutions, LLC",  
    "vendor_url": "https://securityonion.net/",  
    "documentation_url": "https://github.com/Security-Onion-Solutions/security-onion/wiki",  
    "product_name": "Security Onion",  
    "product_url": "https://securityonion.net/",  
    "registry_version": 3,  
    "status": "stable",  
    "maintainer": "Brent Stewart",  
    "maintainer_email": "brent@stewart.tc",  
    "usage": "Your default account will have sudo priviledges.  Squil and Squert username and password are configured in the Setup wizard.  MySQL root is set to null.  For more info see https://github.com/Security-Onion-Solutions/security-onion/wiki/Passwords.",  
    "symbol": "securityonion-logo.png",  
    "qemu": {  
        "adapter_type": "e1000",  
        "adapters": 2,  
        "ram": 3072,  
        "arch": "x86_64",  
        "console_type": "vnc",  
        "kvm": "allow"  
    },  
    "images": [  
        {  
            "filename": "securityonion-16.04.7.1.iso",  
            "version": "16.04.7.1",  
            "md5sum": "6bd811a05c1ec7973b8fca5c34cec13e",  
            "filesize": 2132803584,  
            "download_url": "https://github.com/Security-Onion-Solutions/security-onion/releases/",  
            "direct_download_url": "https://github.com/Security-Onion-Solutions/security-onion/releases/download/v16.04.7.1_20181010/securityonion-16.04.7.1.iso"  
        },  
        {  
            "filename": "securityonion-16.04.6.1.iso",  
            "version": "16.04.6.1",  
            "md5sum": "ca835cef92c2c0daafa16e789c343d1d",  
            "filesize": 2020605952,  
            "download_url": "https://github.com/Security-Onion-Solutions/security-onion/releases/",  
            "direct_download_url": "https://github.com/Security-Onion-Solutions/security-onion/releases/download/v16.04.6.1_20181010/securityonion-16.04.6.1.iso"  
        },  
        {  
            "filename": "securityonion-14.04.5.4.iso",  
            "version": "14.04.5.4",  
            "md5sum": "9c7cab756b675beb10de4274a3ad3bc6",  
            "filesize": 1874853888,  
            "download_url": "https://github.com/Security-Onion-Solutions/security-onion/releases/",  
            "direct_download_url": "https://github.com/Security-Onion-Solutions/security-onion/releases/download/v14.04.5.4_20171031/securityonion-14.04.5.4.iso"  
        },  
        {  
            "filename": "empty30G.qcow2",  
            "version": "1.0",  
            "md5sum": "3411a599e822f2ac6be560a26405821a",  
            "filesize": 197120,  
            "download_url": "https://sourceforge.net/projects/gns-3/files/Empty%20Qemu%30disk/",  
            "direct_download_url": "https://sourceforge.net/projects/gns-3/files/Empty%20Qemu%20disk/empty30G.qcow2/download"  
        }  
    ],  
    "versions": [  
        {  
            "name": "16.04.7.1",  
            "images": {  
                "hda_disk_image": "empty30G.qcow2",  
                "cdrom_image": "securityonion-16.04.7.1.iso"  
            }  
        },  
        {  
            "name": "16.04.6.1",  
            "images": {  
                "hda_disk_image": "empty30G.qcow2",  
                "cdrom_image": "securityonion-16.04.6.1.iso"  
            }  
        },  
        {  
            "name": "14.04.5.4",  
            "images": {  
                "hda_disk_image": "empty30G.qcow2",  
                "cdrom_image": "securityonion-14.04.5.4.iso"  
            }  
        }  
]  
}  

## Testing
In GNS3, go to File > Import Appliance and make sure that your appliance imports correctly.  GNS3 will provide guidance if there's a formatting error.  Looking at the JSON above, you can imagine that a common mistake is unmatched brackets!

If the GNS3a file loads, test it by creating an instance.  You need to test at least any new versions you added.  Make sure the appliance boots without error and that expected interfaces are available.

## Submit a Pull Request
Once the pieces are working, submit the appliance to the community by cloning the GNS3-registry on Github and adding in your file.
> git clone https://github.com/GNS3/gns3-registry.git

If you've _already_ cloned it, make sure that your branch is up to date.  _Upstream_ is the original source (in this case the GNS3 copy).
> git fetch upstream  

Two Python programs are included in the repo.  Run them both on your copy before continuing.  These are QA processes that look for issues before you submit.  They _will_ take a little time to run.
> pip3 install -r requirements.txt   # this does __pip3 install jsonschma__ and __pip3 install pycurl__  
> python3 check.py  
> python3 check_url.py  

Next push your local copy to your github copy.  In Github terms, _origin_ is your copy on Github, and _master_ is the local copy.
> git add .  
> git commit -m "Updated Security Onion"  
> git push -f origin master

Now we have an up to date local copy of the gns3-registry that includes our updated gns3a appliance and we've updated our fork on Github.  Next, we offer our update to the project via a _Pull Request_.  You are going to be one of the cool kids!
![Pull Request](/PullRequest.png#center)
Go to the gns3-registry repository on Github and select the Pull Requests tab and click the big green __New pull request__ button. Under Compare, select the link to _compare across forks_ (since your copy is a fork) and select your fork.  It should show you the changes to files so take a moment to digest that and make sure this PR is doing what you want.  Finally, submit the Pull Request.  Github will email you when there's an update to the request.  If the GNS3 team has a question, they'll submit a comment on the PR and leave it open for you to resolve.  Otherwise, it will get merged in and all the other GNS3 users will be able to enjoy your hard work!  

Thanks!