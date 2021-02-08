## Commands

| Syntax                       | Description                     |
| -----------                  | -----------                     |
| `veracrypt -t -l`            | view mounted volumes            |
| `veracrypt -t -d <path>`     | unmount all (if no path given)  |

## Troubleshooting

!!! error ""
    Go to reference if these occur[^1]

    ``` properties
    lsof: no pwd entry for UID 201 x forever
    mount gives device-mapper: create ioctl failed
    ```

[^1]: https://askubuntu.com/questions/429612/device-mapper-remove-ioctl-on-luks-xxxx-failed-device-or-resource-busy