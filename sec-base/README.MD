## seccurity
security-bootstrapper:
      volumes:
        - name: edgex-init
          #persistentVolumeClaim:
          #  claimName: edgex-init
          hostPath:
            path: "/mnt/edgex/edgex-init"
            type: DirectoryOrCreate

nginx:
      volumes:
        - name: edgex-init
        - name: nginx-templates
        - name: nginx-tls

secret-store:
    depends_on:
      security-bootstrapper:

security-secretstore-setup:
    depends_on:
      secret-store:
      security-bootstrapper:

---

security-proxy-setup:
    depends_on:
      security-bootstrapper:
      security-secretstore-setup:
      volumes:
        - name: edgex-init
        - name: secret-store-config
        - name: nginx-templates
        - name: nginx-tls
        - name: security-proxy-setup-claim5
          hostPath:
            path: "/mnt/edgex/secrets/security-proxy-setup"


mqtt-broker:
    depends_on:
        security-bootstrapper:
        security-secretstore-setup:

database:
    depends_on:
      security-bootstrapper:
      security-secretstore-setup:

