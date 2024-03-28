pipeline {
    environment {
        imagename = "taandav47/blogpost"
        dockerImage = ''
    }
    agent any
    stages {
        stage('Cloning Git') {
            steps {
                    // Ensure we are starting from a clean state
                    bat 'del /s /q *.*'
                    bat 'rmdir /s /q .git'
                                    // Clone the GitHub repository
                    bat 'git clone https://github.com/taandav47/blogpost.git .'
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
    // Create a temporary directory for GitHub Pages deployment
                    bat 'mkdir gh-pages-temp'
                    
                    // Copy static files to the temporary directory
                    bat 'xcopy /s out\\* gh-pages-temp\\'
                    
                    // Create and switch to the gh-pages branch
                    bat 'git checkout --orphan gh-pages'
                    
                    // Clear the contents of the gh-pages branch
                    bat 'git rm -rf .'
                    
                    // Commit empty state
                    bat 'git commit -m "Initial empty commit for GitHub Pages"'
                    
                    // Copy static files to the root directory
                    bat 'xcopy /s gh-pages-temp\\* .'
                    
                    // Add, commit, and push to GitHub Pages branch
                    bat 'git add .'
                    bat 'git commit -m "Deploy to GitHub Pages"'
                    bat 'git push origin gh-pages --force'
                    
                    // Clean up temporary files and switch back to the main branch
                    bat 'rmdir /s /q gh-pages-temp'
                    bat 'git checkout main'
                }
            }
        }
    }
}
