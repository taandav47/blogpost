pipeline {
    environment {
        imagename = "taandav47/blogpost"
        dockerImage = ''
    }
    agent any
    stages {
        stage('Configure Git') {
            steps {
                // Configure Git user name and email
                script {
                    bat 'git config --global user.name "rajat"'
                    bat 'git config --global user.email "acharyarajat1996@gmail.com"'
                }
            }
        }
        stage('Cloning Git') {
            steps {
                script {
                    // Check if 'blogpost' directory exists
                    if (fileExists('blogpost')) {
                        // Delete 'blogpost' directory if it exists
                        bat 'rmdir /s /q blogpost'
                    }
                    // Clone the GitHub repository
                    bat 'git clone https://github.com/taandav47/blogpost.git'
                }
            }
        }
        stage('Building Next.js project') {
            steps {
                bat 'npm install' // Install dependencies
                bat 'npm run build' // Build Next.js project
            }
        }
        stage('Deploy to GitHub Pages') {
            steps {
                script {
                    // Check if 'blogpost' directory exists
                    if (fileExists('gh-pages-temp')) {
                        // Delete 'blogpost' directory if it exists
                        bat 'rmdir /s /q gh-pages-temp'
                    }
                    // Ensure temporary directory exists
                    bat 'mkdir gh-pages-temp'
                    bat 'icacls gh-pages-temp /grant Everyone:F /t'
                }

                // Copy static files to a temporary directory
                bat 'xcopy /s out\\* gh-pages-temp\\'

                // Exclude node_modules from being added to the repository
                bat 'echo node_modules/ >> .gitignore'

                // Delete gh-pages branch if it exists
               bat 'git branch -D gh-pages || exit 0'

                // Create gh-pages branch
                bat 'git checkout -b gh-pages'

                // Clear contents of gh-pages branch
                bat 'git rm -rf .'

                // Commit empty state
                bat 'git commit -m "Initial empty commit for GitHub Pages"'

                // Copy static files to the root directory
                bat 'xcopy /s gh-pages-temp\\* .'

                // Exclude node_modules from being added to the repository
                bat 'echo node_modules/ >> .gitignore'

                // Add, commit, and push to GitHub Pages branch
                bat 'git add .'
                bat 'git commit -m "Deploy to GitHub Pages"'
                bat 'git push origin gh-pages --force'

                // Clean up temporary files and switch back to main branch
                bat 'rmdir /s /q gh-pages-temp'
                bat 'git checkout main'
            }
        }
    }
}
