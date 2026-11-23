# Installing Node.js

The original Codespace we're using doesn't have Node.js installed.

These instructions explain how to get Node.js up and running in our Codespace.

Open `.devcontainer/devcontainer.json`.

Delete its existing contents and replace them with the following:

```json
{
  "name": "PHP + MySQL + Node.js",
  "dockerComposeFile": "docker-compose.yml",
  "service": "app",
  "workspaceFolder": "/workspaces/${localWorkspaceFolderBasename}",

  "features": {
    "ghcr.io/devcontainers/features/node:1": {
      "version": "24"
    }
  },

  "forwardPorts": [8080, 8081, 5173],
  "portsAttributes": {
    "8080": {
      "label": "PHP site (Apache)",
      "onAutoForward": "openPreview"
    },
    "8081": {
      "label": "Adminer",
      "onAutoForward": "notify"
    },
    "5173": {
      "label": "React (Vite)",
      "onAutoForward": "openPreview"
    }
  },

  "customizations": {
    "vscode": {
      "extensions": [
        "bmewburn.vscode-intelephense-client",
        "cweijan.vscode-database-client2"
      ],
      "settings": {
        "editor.formatOnSave": true,
        "workbench.editorAssociations": {
          "\*.md": "vscode.markdown.preview.editor"
        }
      }
    }
  },

  "postCreateCommand": "sudo chmod a+x \"$(pwd)\" && sudo rm -rf /var/www/html && sudo ln -s \"$(pwd)\" /var/www/html",
  "postStartCommand": "apache2ctl start"
}
```

Codespaces should detect the change and ask whether you'd like to rebuild the container.

Select **Rebuild Now**.

When prompted, choose **Rebuild**, not **Full Rebuild**.

To check this has worked

Open the terminal and enter

```
node -v
```

And then

```
npm -v
```

You should get confirmation that Node.js has been installed. 
