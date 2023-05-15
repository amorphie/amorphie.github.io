# Contribution

If you want to develop a project in the Amorphie ecosystem. You can develop according to our folder structure by using the Amorphie template project "https://github.com/amorphie/template". You just need to select this template when creating a new repo, the default project and folder structure used in all amorphie projects will automatically appear. Thus, you will be able to have a standard project structure and you will not have to deal with configurations.

Below you can find detailed explanations of the project and folder structures;
* Template.Core -> Project with our models and common classes
* Template.Data -> Project where db operations are done
* Template.Hub -> Project to manage SignalR notifications if any in the project
* Template.Test -> The project where Architecture Test and Unit Tests will be,
* Template.Worker -> Project where messages from Zeebe or any queue structure will be processed
* Template -> The project's door to the outside world, the part of the project with minimal APIs.
* Dapr -> Dapr component folder
* process -> If the Project is using Zeebe, it contains its BPM files
* prometheus -> prometheus config files
* thunder-tests -> HTTP requests of the project in Vs code in thunder client

The above files and folders have been added to provide a template structure for developing projects in the Amorphie ecosystem.
You can add or subtract according to the needs of the project.I
