## Problem   
Two github accounts, *mahimanWork*(my-work) and *mahiman12*(my personal). I want to use both accounts on the same computer (with use of ssh-keys and not using email and password).
Adding your existing ssh key (for ex. id_rsa.pub, id_ed25519.pub, etc.) in github settings for both of your accounts won't help and will throw an error.

## Solution
We have to use **host name/alias** in the config file (for every account) in ```~/.ssh``` folder to solve the problem

## Solution : Step by Step

1. Check for existing keys : ```ls -al ~/.ssh```

2. Check if you have existing ```.pub``` keys after step 1:
``` 
    id_rsa.pub
    id_ecdsa.pub
    id_ed25519.pub
```
2. If you have an existing key then you have to make another key, otherwise you'll have to create two separate keys :

   1. ```ssh-keygen -t rsa -b 4096 -C "work-email@example.com"```
   2. Enter your ssh-key filename for work id, when prompted ***Enter a file in which to save the key***
   
        ```Enter a file in which to save the key (/Users/you/.ssh/id_rsa): /Users/betterhalf/.ssh/id_work```
    3. Press enter and You will be prompted to Add paraphrases. Add if you want to else press **Enter**
    4. Now, for personal github account, repeat the same process with different email id in the 1st step and in the second step you may use ```/Users/betterhalf/.ssh/id_mahiman``` different file name to save the key
    5. So, now you will have to separate keys for work and personal accounts.

3. Now, check if you have config file present : 
   
   ```open ~/.ssh/config```
   
   else, create a new file

   ```touch ~/.ssh/config```

4. Add host name/alias for your multiple accounts in the newly created config file : ```vim ~/.ssh/config```

    ```
        # Default github account: mahimanWork
        Host github.com
            HostName github.com
            IdentityFile ~/.ssh/id_work
            IdentitiesOnly yes
    
        # Other github account: mahiman12
        Host github-mahiman
            HostName github.com
            IdentityFile ~/.ssh/id_mahiman
            IdentitiesOnly yes
    ```

5. Add ssh private keys to your agent:

    ```
    ssh-add ~/.ssh/id_rsa
    ssh-add ~/.ssh/id_mahiman
    ```

6. Test your connection

    ```
        ssh -T git@github.com
        ssh -T git@github-mahiman.com
    ```

7. If everything is OK, you will get a message for successfull authentication, otherwise you will be prompted if you want to continue connecting. Just type ```yes``` and press **Enter**

8. Now, clone your repository using your host name/alias : ```git@HostName-from-config:org/repository.git```
    
    ```git@github-mahiman.com:mahiman12/git-cmds.git /path/to/directory```

Done! Good to Go! Thank you!
