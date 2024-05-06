# DevOpsTraining
**DevOps/Cloud Training Material**

# Software Development Life Cycle (SDLC) Overview

The Software Development Life Cycle (SDLC) is a process framework used in the development of information systems and software. It describes the stages involved in the creation and maintenance of applications and systems. Below, I briefly outline each phase of the SDLC:

## Phases of the SDLC

1. **Planning:**
   - This initial stage involves defining the objectives of the system or software to be developed. Costs, resources, and a project schedule are evaluated and developed. Proper planning is crucial to align project expectations with desired outcomes.

2. **Requirements Analysis:**
   - During this phase, the needs of the end user are gathered and analyzed to ensure that the system design meets all user needs. It often involves consultation with stakeholders to determine the exact specifications of the software.

3. **Design:**
   - The collected requirements are transformed into a design. The software architecture is decided upon, including technology and data structure, processes, and interface protocols. Models and prototypes can be created.

4. **Implementation or Coding:**
   - This is the process of building the software. Developers begin coding according to the design specifications. It is one of the longest phases of the SDLC.

5. **Testing:**
   - Once the software is developed, it is tested to ensure there are no errors or bugs. Testing can include unit testing, integration testing, system testing, and user acceptance testing.

6. **Deployment:**
   - After the software has passed all tests, it is deployed in the production environment or in the market.

7. **Maintenance and Support:**
   - After deployment, the software requires continuous maintenance to address new errors or to improve its functionality. This phase may include updates and optimizations based on end-user feedback.

8. **Retirement:**
   - Eventually, the system or software may be replaced by a new version or simply retired. Data and processes need to be migrated if necessary.

## The LAMP Stack

The LAMP stack is a set of open-source software technologies used together to host websites and web applications. The acronym LAMP refers to the components that make up this stack:

- **Linux:** the operating system on which all other components run. Linux is chosen for its stability, security, and robustness in server environments.
- **Apache:** the web server used to serve web pages. Apache is popular for its performance and flexibility in configuration. It handles client requests and delivers the appropriate web content.
- **MySQL:** the database management system used to store and manage application data, such as user data, page content, and more. Known for its speed and ease of use.
- **PHP:** the programming language (although Python or Perl can also be used). PHP is widely used to develop server-side scripts that process the business logic of the application.

### Why Use the LAMP Stack?

The LAMP stack is popular due to its performance, cost-effectiveness, and flexibility. Key advantages include:

- **Cost:** All components are open-source software, meaning they are free to use, reducing the total cost of development and maintenance.
- **Community:** Each component of the LAMP stack has an active and extensive community, ensuring continuous support and development.
- **Flexibility:** It is easy to customize. Developers can modify any part of the stack to meet their specific needs.
- **Compatibility:** Highly compatible with a wide range of software and applications, making integration with other tools and systems easy.

### Cloud Implementation

In a cloud environment like AWS, deploying a LAMP Stack can be relatively straightforward using services such as Amazon EC2 for computing, Amazon RDS for managing MySQL databases, and AWS Elastic Beanstalk for application management, providing scalability and efficient infrastructure management.

## Managing File Permissions and Ownership

### chmod Command

The `chmod` (change mode) command is used to change the access permissions of files and directories. For example, to change the permissions of a file named `example.txt` to 755, you would use:

```bash
chmod 755 example.txt
```

### chown Command

The `chown` (change owner) command is used to change the ownership of a file or directory. For example, if you want to change the owner of the file `example.txt` to the user "user" and the group "group", you would execute:

```bash
chown user:group example.txt
```


