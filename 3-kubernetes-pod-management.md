💻 Interactive Container Execution (exec)
To tunnel into a running container environment for real-time debugging, environment verification, or internal configuration inspection:

# Open an interactive pseudo-TTY bash session inside the target Pod
kubectl exec -it firstpod -- bash

# Example workflow inside the container:
root@firstpod:/# nginx -v
root@firstpod:/# exit

-i (--stdin): Keeps the standard input stream open even if not attached.

-t (--tty): Allocates a pseudo-TTY terminal screen interface.

-- bash: Specifies the target shell command interpreter to invoke within the container runtime.

🔍 Detailed Pod Inspection (describe)
When debugging runtime errors, image pull issues, or health check failures, use the low-level description flag to inspect lifecycle events and container configurations:

# Fetch comprehensive runtime metadata, status conditions, and recent cluster events
kubectl describe pod firstpod

Key Areas to Monitor in the Output:
Status / Phase: Verifies if the Pod is Pending, Running, Succeeded, or Failed.

Events: A chronological log at the bottom displaying critical steps like scheduling, volume mounting, image pulling, and container starts.

Containers: Lists port mappings, environment variables, mounts, and state tracking.
