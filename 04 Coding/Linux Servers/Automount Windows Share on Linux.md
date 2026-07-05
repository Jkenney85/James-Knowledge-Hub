
1. Need to install CIFS tools to mount and automount a Windows Shared Drive
    
    - Sudo apt install cifs-utils
    
2. Create a mount point
    
    - This can be in the user's home directory or in the root, I choose to put it in my user's home directory ex:  
        /home/ubuntu/Media
    
3. Not sure if the following really helps or not but I've done it anyways just in case.
    
    - Sudo chown -R nobody:nogroup /home/ubuntu/Media
    - Sudo chmod -R 0755 /home/ubuntu/Media        
    
4. Setup the automount
    
    - Create a windows credentials file
        
        - Sudo nano /etc/win_cred
            
            1. Insert the following  
                username=windows_user_name  
                password=windows_password
                
            2. Save and exit
                
            
        - Sudo chown root: /etc/win_cred
            
        - Sudo chmod 600 /etc/win_cred
            
        
    - Setup fstab for automounting the share
        
        - Sudo nano /etc/fstab
            1. Insert the following
                
                - //10.0.0.201/Media /home/ubuntu/Media cifs credentials=/etc/win_cred,iocharset=utf8,sec=ntlmssp,uid=1000,gid=1000 0 0
                
            2. Save and exit
                
    
5. Test mounting
    
    - Sudo mount /home/ubuntu/Media
        
    - Sudo umount /home/ubuntu/Media
        

If successful this should allow the host(ubuntu server) read/write files to the share as well as docker containers running on the host