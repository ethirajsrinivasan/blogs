## JDK variants: From Sun MicroSystems to Oracle and Beyond
Java was originally developed by Sun Microsystems, a technology company that was founded in 1982. Sun Microsystems released the first version of Java in 1995, and it quickly gained popularity as a versatile and platform-independent programming language.

![Java Developer By Reka Illyes From Unsplash](https://images.unsplash.com/photo-1617729420692-219af0d56dae)

In 2010, Oracle Corporation, another technology company, acquired Sun Microsystems, including its Java-related assets, as part of a business acquisition. This included the ownership of Java, including its intellectual property, trademarks, copyrights, and other related assets.

After the acquisition, Oracle became the new owner and steward of Java, taking over the responsibility for its development, maintenance, and support. Oracle continued to invest in Java, providing updates, enhancements, and new versions of the Java platform, including the Java Development Kit (JDK) and Java Runtime Environment (JRE).

Since then, Oracle has been actively involved in the development and evolution of Java, contributing to the Java Community Process (JCP) and providing leadership in shaping the future direction of Java. Oracle has also been responsible for the distribution of Oracle JDK, which is one of the widely used distributions of the JDK.

<h3 align="center">* * * </h3>

As of April 16, 2019, Oracle changed its licensing and distribution model for the Java Development Kit (JDK) and Java Runtime Environment (JRE), which includes Oracle JDK. This change was implemented with the release of JDK 11, and subsequent versions of Oracle JDK, where Oracle implemented a new release cycle and support model for Java.

![Java Release Cycle From Oracle](https://user-images.githubusercontent.com/7569031/232728161-11cc613e-ec08-46d9-a312-9f92f842a72c.png)


Oracle JDK is no longer free for commercial use primarily due to the following reasons:
* **Oracle’s business model**
* **Shifting focus to long-term support (LTS) releases**
* **OpenJDK as the open-source alternative**

**Oracle's business model**
Oracle, like many other technology companies, generates revenue from licensing and support services. By making changes to the licensing and distribution model of Oracle JDK, Oracle aimed to monetize and generate revenue from their Java offerings, particularly for commercial use.

**Shifting focus to long-term support (LTS) releases**
With the introduction of the new release cycle and support model, Oracle shifted its focus to Long-Term Support (LTS) releases, which are designated every three years. LTS releases receive extended support and updates for a longer duration compared to non-LTS releases. As part of this change, Oracle made updates and patches for older JDK versions, including JDK 8 (which was previously free for commercial use), available only to customers with a commercial support subscription.

**OpenJDK as the open-source alternative**
Oracle actively supports and contributes to the OpenJDK project, which is the open-source reference implementation of the Java SE platform. OpenJDK is available under an open-source license, such as the GNU General Public License (GPL) or the GNU Lesser General Public License (LGPL), which allows for free use, modification, and distribution of the software. Oracle encourages users to use OpenJDK as a free and open-source alternative to Oracle JDK for commercial use.

<h3 align="center">* * * </h3>

It's important to note that while Oracle JDK is no longer free for commercial use, there are other alternatives available, such as OpenJDK, which is widely used and supported by the Java community. Additionally, some other companies and organizations, such as Amazon with their Amazon Corretto JDK, offer free, open-source distributions of the JDK that can be used for commercial purposes. It's crucial to review and comply with the licensing and usage terms of each JDK distribution to ensure compliance with their respective licensing policies. Lets looks about more about the following JDKs.

* **Oracle JDK**
* **OpenJDK**
* **Amazon Corretto**
* **AdoptOpenJDK (Eclipse Temurin) **
* **Zulu JDK**
* **GraalVM**

![image](https://user-images.githubusercontent.com/7569031/232730798-629e6645-f6e4-4f38-8817-0424e86ef4e4.png)


**Oracle JDK** 

This is the official JDK distribution provided by Oracle, the original creator and maintainer of Java. It has a long history and has been widely used in many Java applications. It includes features such as Java Flight Recorder for monitoring and diagnostics, and Java Mission Control for profiling and tuning Java applications. However, starting with Java 11, Oracle JDK is no longer free for commercial use and requires a paid subscription for extended support. It is recommended for applications that require official support from Oracle and need to use the additional tools provided by Java Flight Recorder and Java Mission Control. To install Oracle JDK, you can follow these general steps:

* For Ubuntu or Debian-based systems, you can download the Oracle JDK installer from the official Oracle JDK website (https://www.oracle.com/java/technologies/javase-jdk14-downloads.html) and follow the installation instructions.

* For CentOS or Red Hat-based systems, you can download the Oracle JDK installer from the official Oracle JDK website and follow the installation instructions.

* For Windows or macOS, you can download the Oracle JDK installer from the official Oracle JDK website and follow the installation instructions.

**OpenJDK**

This is the open-source reference implementation of the Java SE specifications, and it is maintained by a global community of developers. OpenJDK is free to use and provides the core Java libraries, compiler, and virtual machine. It serves as the basis for many other JDK distributions and is widely used in production environments. It is a good option for applications that prioritize open-source software, community-driven development, and adherence to Java standards. To install OpenJDK, you can follow these general steps:

* For Ubuntu or Debian-based systems:
```bash
sudo apt-get update
sudo apt-get install openjdk-11-jdk
```

* For CentOS or Red Hat-based systems:
```bash
sudo yum install java-11-openjdk-devel
```

* For Windows or macOS, you can download the OpenJDK installer from the official OpenJDK website (https://jdk.java.net/).


**Amazon Corretto**

This is a no-cost, multiplatform, production-ready distribution of OpenJDK provided by Amazon. It includes long-term support (LTS) releases and is optimized for running Java applications on Amazon Web Services (AWS) cloud infrastructure. It also provides additional features for seamless integration with AWS services, such as Amazon CloudWatch for monitoring and AWS X-Ray for tracing. It is recommended for Java applications that are specifically designed to run on AWS and require optimized performance and integration with AWS services. To install Amazon Corretto, you can follow these general steps:

* For Ubuntu or Debian-based systems:
```bash
sudo apt-get update
sudo apt-get install amazon-corretto-11
```

* For CentOS or Red Hat-based systems:
```bash
sudo yum install amazon-corretto-11
```

* For Windows or macOS, you can download the Amazon Corretto installer from the official Amazon Corretto website (https://aws.amazon.com/corretto/).


**AdoptOpenJDK (Eclipse Temurin)**

This is a eclipse foundation effort to provide free, open-source distributions of OpenJDK with timely updates, long-term support options, and a wide range of platforms and architectures supported. It provides features such as timely updates, support for various platforms, and performance optimizations. It is a popular choice for applications that require a free and open-source JDK with options for long-term support and a wide range of platform support. To download Adoptium JDK (formerly known as AdoptOpenJDK) from Temurin, follow these steps:

* Go to the Temurin website: https://adoptium.net/
* Click on the “Download” button in the top navigation menu.
* Select the version of the JDK you want to download from the dropdown menu. You can choose from the latest LTS release, the latest feature release, or a specific version.
* Choose the operating system and architecture you want to download for. The options available will depend on the version of the JDK you selected.
* Click on the “Download” button for the appropriate package.
* You will be redirected to a page with a list of mirrors where you can download the JDK. Choose a mirror closest to your location and click on the link to download the JDK package.
* Once the download is complete, extract the contents of the package and follow the installation instructions for your operating system.

**Zulu JDK**

This is a distribution of OpenJDK provided by Azul Systems, a company that specializes in Java runtime solutions. It includes free and commercial options, with features such as timely updates, support for various platforms, and performance optimizations. It also provides additional tools and features such as Zulu Mission Control for profiling and monitoring Java applications. It is recommended for applications that require performance optimizations and additional tools provided by Zulu Mission Control. To install Zulu JDK, you can follow these general steps:
* Go to the Azul Systems website (https://www.azul.com/downloads/zulu-community/) for Zulu JDK Community edition or (https://www.azul.com/downloads/zulu-enterprise/) for Zulu JDK Enterprise edition.
* Select the desired JDK version, type (e.g., HotSpot or OpenJ9), and operating system.
* Click on the download link to download the installer.
* Run the installer and follow the on-screen instructions to install Zulu JDK.

**GraalVM**

This is a high-performance runtime that provides a polyglot environment for running Java, JavaScript, LLVM-based languages, and other languages. It includes a distribution of OpenJDK and provides features such as ahead-of-time (AOT) compilation and just-in-time (JIT) compilation optimizations. GraalVM is known for its performance optimizations and support for multiple programming languages, making it suitable for applications that require polyglot capabilities and high-performance execution. To install GraalVM, you can follow these general steps:
* Go to the GraalVM website (https://www.graalvm.org/downloads/) and click on the "Download" button.
* Select the desired JDK version and operating system.
* Click on the download link to download the GraalVM distribution.
* Extract the downloaded distribution to a desired directory.


It's important to carefully evaluate the features, licensing, support, performance, and ecosystem aspects of different JDK distributions based on your specific requirements and use case. The "best" JDK distribution may vary depending on factors such as licensing considerations, support needs, performance optimizations, and community preferences. It's recommended to thoroughly research and choose the JDK distribution that best fits your particular requirements.



