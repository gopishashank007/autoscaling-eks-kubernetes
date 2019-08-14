Modify the Min/Max Sizes of the Autoscaling Group

    In your browser, log in to the AWS Management Console with the credentials provided on the lab instructions page. Make sure you are using the us-east-1 (N. Virginia) region.
    Navigate to the EC2 service.
    Click Autoscaling Groups in the left sidebar.
    Select the autoscaling group that has already been created for you.
    Note the name of the autoscaling group (we'll need it later).
    Click Actions > Edit.
    In the Edit details menu, configure the following settings:
        Min: 2
        Max: 8
    Click Save.

Configure the Cluster Autoscaler

    Go back to your terminal application
    List the contents of the home directory.

    ls

    Edit the cluster_autoscaler.yaml file.

    vim cluster_autoscaler.yaml

    Locate the <AUTOSCALING GROUP NAME>placeholder, and replace it with the autoscaling group name we found in the AWS Management Console.
    Press Escape, then type :wq to quit the vim text editor.

Apply the IAM Policy to the Worker Node Group Role

    List the contents of the asg-policy.json file.

    cat asg-policy.json

        Copy the content of asg-policy.json to your clipboard.
        Switch to the AWS Management Console.
        Navigate to the IAM service.
        Click Roles in the left sidebar.
        Type "node" in the search bar.
        Click the name of the role that appears in the search results to open it.
        Click + Add inline policy.
        Click the JSON tab.
        Delete the default text from the policy editor, and paste in the content of asg-policy.json you copied to your clipboard earlier.
        Click Review policy.
        Name the policy "CA".
        Click Create policy.

Deploy the Cluster Autoscaler

    Go back to your terminal application.
    Run the following command:

    kubectl apply -f cluster_autoscaler.yaml

    Check the cluster autoscaler logs.

    kubectl logs -f deployment/cluster-autoscaler -n kube-system

    Press Ctrl + C to exit the logs.

Deploy and Scale the Nginx Deployment
Deploy a Sample Nginx Application

    List the contents of the current directory.

    ls

    List the contents of the nginx.yaml file.

    cat nginx.yaml

    Deploy the nginx deployment.

    kubectl apply -f nginx.yaml

    Verify that the deployment was successful.

    kubectl get deployment/nginx-scaleout

Scale the Nginx Deployment

    Run the following command:

    kubectl scale --replicas=10 deployment/nginx-scaleout

    Check the autoscaler logs again.

    kubectl logs -f deployment/cluster-autoscaler -n kube-system

    Go back to the AWS Management Console.
    Navigate to the EC2 service.
    Click 3 Running Instances.
    Go back to your terminal application.
    Check the nodes.

    kubectl get nodes

    Delete the nginx and cluster autoscaler deployments.

     kubectl delete -f cluster_autoscaler.yaml
     kubectl delete -f nginx.yaml

