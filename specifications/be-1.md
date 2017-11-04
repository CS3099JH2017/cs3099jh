# CS3099 - Basic Specification
## 'Backend' Groups
## Ognjen Arandjelović (Oggie)\*

### Overview

The participants of CS3099 have been tasked with designing and creating a data hosting and analysis system for the University's biomedical department. The task has been divided into 3 'sections':

 1. Backend
 2. Machine Learning and Data Analysis
 3. Visualisation and Human-Computer Interaction

Several groups have been assigned to each section, and the best product from each section will be used in combination as the final system.

### Initial Deliverable

Project- and user-management server, professionally developed, presented, and documented.
 - Data should be stored in discrete _projects_.
   - The backend should treat this data as agnostic (short of discriminating based on file format).
     - Ideally, this service should be useful for any project-management system.
     - The database should be aware of dependencies between files, and should act appropriately if a user attempts to delete a file depended on.
   - This data may be images or text.
   - The backend should handle the conversion of files between common file formats.
     - For images, formats such as _Leica_, _Zeiss_, and _TIFF_ should be included as supported file types (these are common output formats of devices such as medical microscopes).
     - For text, formats such as _CSV_ and _XLS_ should be supported.
     - The ML and HCI groups may also request other file formats, either for basic functionality or for extension work. Make sure to cater to as many other groups as possible!
   - Folders within projects should be possible, for grouping of relevant data.
 - A user management system should be used to control actions on projects/data.
   - Creating, viewing and removal of projects should also be accounted for.
   - Adding, viewing, editing and removing of files within a project should be actions controlled by permissions given to users on a per-project basis.
   - Adding, viewing, editing, and removing users (e.g. admin or superuser abilities) should be controlled by user permissions.
 - The server should provide service through one or more APIs.
   - Discuss with other groups about important interactions between their service(s) and yours, and design with other BE groups a common API for other groups to use.

### Additional Points of Interest

- The specifications for the other two groups give extra context for the project as a whole.
- There will be opportunities for further context and clarification regarding the project as a whole: contact with the client is possible and encouraged. The client is invested in the outcome of the project and is available for assistance.
- Extensions on this specification, including more advanced requirements, can be requested after this initial specificatoin is satisfied.
- You may find it useful, as a means of inspiration, to have a look at the following applications which were developed in the same application realm but do not share or fully meet our client’s needs:
   - https://qupath.github.io/ and https://www.biorxiv.org/content/early/2017/01/12/099796
   - http://cellprofiler.org/cp-analyst/
   - http://www.tmanavigator.org/
   - https://spotfire.tibco.com/

\*Revised by JH Project Paticipants
