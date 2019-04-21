## When you see the error of "pip docker and docker-py have same module you have remove one of them then, 
The following should fix your issue:

```bash
pip uninstall docker
pip uninstall docker-py
pip uninstall docker-compose
pip install docker-compose==1.9.0
```

