# configmap
```
- name: APP_TEST_NAME
          valueFrom:
            configMapKeyRef:
              # The ConfigMap containing the value you want to assign to SPECIAL_LEVEL_KEY
              name: test-config
              # Specify the key associated with the value
              key: app_test_name
```
# configmap all
```
envFrom:
        - configMapRef:
            name: test-config
```

# secret
```
- name: APP_TEST_DB_PASS
          valueFrom:
            secretKeyRef:
              # The ConfigMap containing the value you want to assign to SPECIAL_LEVEL_KEY
              name: app-secret
              # Specify the key associated with the value
              key: db_pass
```

# secret all
```
envFrom:
        - secretRef:
            name: app-secret
```