In this exercise you will make a change in the source code and rebuild/re-deploy the container.

# Change the application source code 

You can either make a source code change using an editor or you can use the below ``sed`` command.

First, make sure you are in the correct source code directory:

```execute
cd ~/flask-vote-app
```

<!--
If you want to use an editor, you could use the ``nano`` editor to change the title of the voting definition (e.g. "Linux distribution") and then save the file again:

```execute
nano seeds/seed_data.json
```
Once you have made the change, save the file using ``CTL-X``, then ``Y`` and then hit ``ENTER``.  
-->

Use the following command to substitute some text in the voting definition file for you:

```execute
sed -i "s/distribution/Distribution/" seeds/seed_data.json
```

You have just made a change to the source code. You have changed the word to "``Distribution``".  As a developer, you would now want to build and 'test' the application on your 'local workstation' and after you are happy that it works you would ``commit`` and ``push`` the changes to GitHub. 

Check the changes you made with git:

```execute
git status
```

Git should show you that you made a change in the seed_data.json file:
 - ``modified:   seeds/seed_data.json``.

Git can also show you the exact changes you are about to commit:

```execute
git diff seeds/seed_data.json
```

# Commit and push the changes 

Commit the changes: 

```execute
git commit -m "Important change" . 
```

 - ``After the next command, if you had set up the GitHub webhook in the previous exercise, you should see the source to image build starting automatically.  You will see a build pod running in the lower terminal.``

Now, push the changes to GitHub:

```execute
git push 
```

``If you set up a Webhook`` in the previous exercise, you should see the build automatically starting. Take a look at the lower terminal window and you should see ``vote-app-2-build    1/1     Running``. 



 - ``Warning: If you DID NOT set up a webhook in the previous exercise, trigger a new build manually with the following command``: 

```execute
# ONLY RUN IF YOU DID NOT SET UP THE WEBHOOK IN THE PREVIOUS LAB
oc start-build vote-app   
```

Ensure the build is running before continuing. 

Again, follow the build progress:

```execute
oc logs bc/vote-app --follow
```

You should see the python application being built again and a new image being committed/pushed into the internal registry. The application should be re-deployed automatically.   Note, this can take a few minutes, especially the ``Copying blob...`` and the ``Storing signatures`` operations can be slow. 

Once the new pod is up and running, check the change you just made has been deployed properly. 

# Verify the application changes 

```execute 
curl -s http://vote-app-%project_namespace%.%cluster_subdomain%/ | grep "<title>"
```

The output should include your change, for example:

```
    <title>Favourite Linux Distribution</title>
```

 - ``If this does not work, go back and check that the build had finished successfully and that the application has been re-deployed.``

Test the application in a browser with the following URL:

[Open the Vote Application](http://vote-app-%project_namespace%.%cluster_subdomain%/)


---
That's the end of this exercise.

You now know how to make changes to your code and test the results.  Next you will connect the vote application to a database. 


