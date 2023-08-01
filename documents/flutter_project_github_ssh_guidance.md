
# Flutter Project GitHub SSH Guidance

In this guide, we'll walk you through the process of setting up SSH for your Flutter project hosted on GitHub, including generating SSH keys, adding them to your GitHub account, and adding GitHub-hosted packages to your Flutter project.

## Generating SSH Keys

1. Open the terminal on your developer's computer.
2. Run the following command to generate a new RSA 4096-bit SSH key pair:

   ```bash
   ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
   ```

   Replace "<your_email@example.com>" with your own email address.

3. The terminal will prompt you to choose where to save the SSH key. By default, it's usually saved in `/home/yourusername/.ssh/id_rsa`. You can specify a different path if you prefer.

## Adding SSH Key to GitHub

1. Log in to your GitHub account.
2. Click on your profile picture in the top right corner, and select "Settings" from the dropdown menu.
3. Navigate to "SSH and GPG keys" in the left sidebar.
4. Click on "New SSH key".
5. Give your key a title, and paste the content of the `id_rsa.pub` file generated in the previous step (you can view the content using the `cat ~/.ssh/id_rsa.pub` command).
6. Click "Add SSH key" to save it.

## Cloning a Flutter Project from GitHub

1. In the terminal, use the following command to clone the project:

   ```bash
   git clone git@github.com:username/repo.git
   ```

   Replace "username" with your GitHub username and "repo" with the name of the repository you want to clone.

2. After cloning is completed, the project will be downloaded to your local computer, and you can start working on it.

## Adding GitHub-hosted Packages to Flutter Project

1. Open the project and edit the `pubspec.yaml` file.
2. Add the GitHub-hosted packages to the `dependencies` section. For example:

   ```yaml
   dependencies:
     package_name:
       git:
         url: git@github.com:username/package_name.git
   ```

   Replace "package_name" with the name of the package and "username" with your GitHub username.

3. Optionally, you can use the `ref` parameter to specify a version, branch, commit hash, or tag. For example:

   ```yaml
   dependencies:
     package_name:
       git:
         url: git@github.com:username/package_name.git
         ref: main
   ```

   This example assumes you want to use the "main" branch. You can use other references as needed.

4. After making changes to `pubspec.yaml`, run the following command in the terminal to update the packages:

   ```bash
   flutter pub get
   ```

   This command will download the newly added GitHub packages and resolve dependencies.

You've now set up SSH for your Flutter project hosted on GitHub and added GitHub-hosted packages to your project. Your developer can start working with the project and use the private packages from GitHub in their Flutter app.
