# Red Hat Process Automation Manager 7 OpenShift images

This repository contain all the image descriptors and files necessary to build the RHPAM images.
It also includes the application templates, however they are deprecated in 7.11. Using the Red Hat Business Automation Operator is recommended.


### Repo structure:

**rhpam-7-openshift-image** \
├── **[businesscentral](businesscentral)**: Business Central image descriptor files.\
├── **[businesscentral-monitoring](businesscentral-monitoring)**: Business Central Monitoring image descriptor files. \
├── **[controller](controller)**: RHPAM Controller  image descriptor files. \
├── **[kieserver](kieserver)**: Execution Server image descriptor files \
├── **[quickstarts](quickstarts)**: quickstarts used as example in some of the application templates. \
├── **[smartrouter](smartrouter)**: RHPAM Smart Router image descriptor files. \
└── **[templates](templates)**: Application templates, used to instantiate a new RHPAM environment on OpenShift. (Deprecated) \
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;├── [contrib](templates/contrib) \
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;│&nbsp;&nbsp;&nbsp;&nbsp;└── [jdbc](templates/contrib/jdbc): Examples about how to add third party jdbc driver on RHPAM images. \
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└── [docs](templates/docs):  Application templates docs generated by the [gen_template_docs.py](https://github.com/jboss-container-images/jboss-kie-modules/blob/master/tools/gen-template-doc/gen_template_docs.py) script. \

Inside each image directory you will find the following files:

 - **container.yaml**: used by OSBS builds.
 - **content_sets.yaml**: define the yum repositories needed to install dependencies for the image.
 - **branch-overrides.yaml**: overrides file which uses the latest stable version for external dependencies. The module.yaml and image.yaml always refer to master branch.
 - **tag-overrides.yaml**: Used to override the branchs to use the final tags to rebuild released images, for CVE respins.
 - **image.yaml**: the main image descriptor file, here are all the pieces and configuration needed to build an image.


On the root directory you can also find the two files:
 - [example-app-secret-template.yaml](example-app-secret-template.yaml): Contains a https certificate to be used as example where https is required.
 - [rhpam-image-streams.yaml](rhpam711-image-streams.yaml): imagestreams definitions, file used to install the product image streams on OpenShift, this files contains the image stream name and the registry the image will be pulled of.


This repo depends directly on 5 repositories, which are:
 - [cct_module](https://github.com/jboss-openshift/cct_module.git): contains some core modules, like s2i and maven.
 - [jboss-eap-modules](https://github.com/jboss-container-images/jboss-eap-modules.git): contains modules responsible to configure JBoss EAP, like datasources and the base configuration files.
 - [jboss-eap-image](https://github.com/jboss-container-images/jboss-eap-7-image.git): provides the EAP modules, which is used to install the EAP on the target image.
 - [rhpam-7-image](https://github.com/jboss-container-images/rhpam-7-image.git): provides the RHPAM binaries modules.
 - [jboss-kie-modules](https://github.com/jboss-container-images/jboss-kie-modules): contains all the resources used to configure the RHPAM images.

#### Building Images

To build the images for local testing there is a [Makefile](./Makefile) which will do all the hard work for you.
With this Makefile you can:

- Build and test all images with only one command:

     ```bash
     make
     ```
     If there's no need to run the tests just set the *ignore_test* env to true, e.g.:

     ```bash
     make ignore_test=true
     ```

- Test all images with only one command, no build triggered, set the *ignore_build* env to true, e.g.:

     ```bash
     make ignore_build=true
     ```

- Build images individually, by default it will build and test each image

     ```bash
     make image image_name=businesscentral
     make image image_name=businesscentral-monitoring
     make image image_name=controller
     make image image_name=dashbuilder
     make image image_name=kieserver
     make image image_name=process-migration
     make image image_name=smartrouter
     ```
  
     We can ignore the build or the tests while interacting with a specific image as well, to build only:

     ```bash
     make ignore_test=true image_name={image_name}

     ```

     Or to test only:

     ```bash
     make ignore_build=true image_name={image_name}
     ```

- List all available Images:
    To list all available images in the reposotiory do:

    ```bash
    make list-images
    ```

##### Found a issue?
Please submit a issue [here](https://issues.jboss.org/projects/RHPAM) with the **Cloud** tag or send us a email: bsig-cloud@redhat.com.
