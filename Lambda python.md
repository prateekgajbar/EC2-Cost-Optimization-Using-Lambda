```
import boto3
import logging

logger = logging.getLogger()
logger.setLevel(logging.INFO)

ec2 = boto3.resource('ec2')

def lambda_handler(event, context):
    logger.info(f"Event received: {event}")

    action = event.get('action')
    instance_name = event.get('instance_name')

    if not action or not instance_name:
        return {
            'statusCode': 400,
            'message': 'Missing action or instance_name'
        }

    filters = [
        {'Name': 'tag:Name', 'Values': [instance_name]}
    ]

    instances = list(ec2.instances.filter(Filters=filters))
    instance_ids = [i.id for i in instances]

    logger.info(f"Matched instances: {instance_ids}")

    if not instance_ids:
        return {
            'statusCode': 200,
            'message': f'No matching instances for {instance_name}'
        }

    if action == 'start':
        ec2.instances.filter(InstanceIds=instance_ids).start()
    elif action == 'stop':
        ec2.instances.filter(InstanceIds=instance_ids).stop()
    else:
        return {
            'statusCode': 400,
            'message': 'Invalid action'
        }

    return {
        'statusCode': 200,
        'message': f'{action.upper()} successful for {instance_name}',
        'instance_ids': instance_ids
    }
