## Troubleshooting
!!! error ""
    If you get;
    ``` properties
    chsh: PAM authentication failed
    ```

    Find and comment this line inside `etc/pam.d/chsh`.
    ``` properties
    auth required pam_shells.so
    ```

    Then do whatever you were doing and make sure to **uncomment it again**.



[^1]: https://ubuntuforums.org/showthread.php?t=1702833
