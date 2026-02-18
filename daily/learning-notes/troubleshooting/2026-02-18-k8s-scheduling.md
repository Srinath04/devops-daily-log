# K8s Pod Scheduling Debug
## Problem
Pod stuck in Pending state with FailedScheduling errors

## Root Causes
1. nodeSelector required label was not found with nodes
2. Memory request was huge about 2000Mi which exceeded node capacity

## Fix
- Removed nodeSelector from manifest
- Reduced memory limits/requests to 128Mi

## Result
Pod scheduled successfully, nginx accessible via LoadBalancer

## Lesson
Always check node labels and available resources when debugging scheduling failures

