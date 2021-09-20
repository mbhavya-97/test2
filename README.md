# test2
name: Test Environments
on:
  push:
    branches:
    - master
env:
  PROJECT: test
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - name: Create & Zip artifact
      run: | 
        echo "Stuff" > test.txt 
    - name: Publish Artifacts
      uses: actions/upload-artifact@v2.2.2
      with:
        name: webapp
        path: test.txt
  development:
    needs: build
    environment:
     name: develop
     url: https://develop.test.com
    runs-on: ubuntu-latest
    steps:
    - name: Download a Build Artifact
      uses: actions/download-artifact@v2.0.8
      with:
        name:  webapp
    - name: Development Deployment
      run: | 
        echo  ${{ secrets.PASSWORD }}
        echo "Development Deployment"
  production:
    needs: development
    environment:
     name: production
     url: https://test.com
    runs-on: ubuntu-latest
    steps:
    - name: Download a Build Artifact
      uses: actions/download-artifact@v2.0.8
      with:
        name:  webapp
    - name: Production Deployment
      run: | 
        echo  ${{ secrets.PASSWORD }}
        echo "Production Deployment"
