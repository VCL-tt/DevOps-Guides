# Guía de Comandos para usar el servicio EKS: 

## Paso 1: Crear un Host de Gestión EKS en AWS

### Lanzar una Nueva VM Ubuntu usando AWS EC2

1. **Lanzar una nueva instancia EC2**:
   - Elige **Ubuntu** como sistema operativo.
   - **Tipo de instancia**: t2.micro (o el tipo que prefieras).
   
2. **Guardar la clave privada**:
   - Cuando crees la instancia EC2, asegúrate de guardar la **clave privada** (.pem) en una carpeta segura de tu computadora. La clave se utilizará para autenticarte en la instancia.
   
   - **Es importante**: Cambia los permisos de la clave privada para garantizar que no se pueda ver públicamente:
     ```bash
     chmod 400 "YOUR KEY.PEM"
     ```

3. **Conectar a la instancia usando SSH**:
   - Abre **Git Bash** (o cualquier terminal que soporte comandos Linux).
   - Navega a la carpeta donde guardaste tu archivo `.pem` (por ejemplo, `EKSKey.pem`).
   - Usa el siguiente comando para conectarte a tu instancia EC2:
     ```bash
     ssh -i "EKSKey.pem" ubuntu@ec2-3-21-104-225.us-east-2.compute.amazonaws.com
     ```

   - **Nota**: Asegúrate de que la clave privada `.pem` esté en la misma carpeta desde donde ejecutas el comando o especifica la ruta completa del archivo `.pem` en el comando.

4. **Instalar kubectl**:
   - Ejecuta los siguientes comandos para instalar **kubectl**:
     ```bash
     curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.19.6/2021-01-05/bin/linux/amd64/kubectl
     chmod +x ./kubectl
     sudo mv ./kubectl /usr/local/bin
     kubectl version --short --client
     ```

5. **Instalar AWS CLI**:
   - Ejecuta los siguientes comandos para instalar la última versión de **AWS CLI**:
     ```bash
     sudo apt install unzip
     curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
     unzip awscliv2.zip
     sudo ./aws/install
     aws --version
     ```

6. **Instalar eksctl**:
   - Ejecuta los siguientes comandos para instalar **eksctl**:
     ```bash
     curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
     sudo mv /tmp/eksctl /usr/local/bin
     eksctl version
     ```

---

## Paso 2: Crear un Rol IAM y Adjuntarlo al Host de Gestión EKS

1. **Crear un nuevo rol IAM**:
   - Ve al servicio **IAM** en AWS.
   - Selecciona **Caso de uso**: **EC2**.

2. **Añadir permisos**:
   - Adjunta los siguientes permisos para el rol:
     - **AdministratorAccess**.

3. **Nombre del rol**:
   - Nombra el rol como prefieras, ejemplo: `eksroleec2`.

4. **Adjuntar el rol al Host de Gestión EKS**:
   - Ve a **EC2** en la consola de AWS.
   - Selecciona tu instancia.
   - Haz clic en **Seguridad**, luego en **Modificar rol IAM**.
   - Adjunta el rol IAM `eksroleec2` que creaste anteriormente.

---

## Paso 3: Crear un Clúster EKS usando eksctl

1. **Sintaxis para crear el clúster**:

   Para crear un clúster EKS, ejecuta el siguiente comando:
   ```bash
   eksctl create cluster --name <nombre-del-cluster> --region <nombre-de-la-region> --node-type <tipo-de-instancia> --nodes-min 2 --nodes-max 2 --zones <zonas-de-disponibilidad>
2. Ejemplos clúster:
    
   - Ejemplo para Virginia del Norte (us-east-1):
      ```bash
     eksctl create cluster --name mi-cluster-virginia --region us-east-1 --node-type t2.micro --nodes-min 2 --nodes-max 2 --zones us-east-1a,us-east-1b
    - Ejemplo para  Ohio (us-east-2):
      ```bash
      eksctl create cluster --name mi-cluster-ohio --region us-east-2 --node-type t2.micro --nodes-min 2 --nodes-max 2 --zones us-east-2a,us-east-2b
    - Ejemplo para Mumbai (ap-south-1):
      ```bash
      eksctl create cluster --name mi-cluster-mumbai --region ap-south-1 --node-type t2.micro --nodes-min 2 --nodes-max 2 --zones ap-south-1a,ap-south-1b
    - Ejemplo para Europa (Irlanda - eu-west-1):
      ```bash
      eksctl create cluster --name mi-cluster-europa --region eu-west-1 --node-type t2.micro --nodes-min 2 --nodes-max 2 --zones eu-west-1a,eu-west-1b
    - Ejemplo para Sídney (ap-southeast-2):
      ```bash
      eksctl create cluster --name mi-cluster-sidney --region ap-southeast-2 --node-type t2.micro --nodes-min 2 --nodes-max 2 --zones ap-southeast-2a,ap-southeast-2b
