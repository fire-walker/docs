## Troubleshooting

When faced with this error [1] <br>
`chsh: PAM authentication failed`

Find and comment the below 
```
#from
/etc/pam.d/chsh
#comment
auth required pam_shells.so
```

Rerun <br>
`chsh -s /bin/false username`

Uncomment the above


## References

[1] - https://ubuntuforums.org/showthread.php?t=1702833
