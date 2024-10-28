import json
import boto3
from botocore.exceptions import ClientError
from decimal import Decimal

# Initialize DynamoDB resource
dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table('studentsCreation')

# Custom JSON encoder for Decimal objects
class DecimalEncoder(json.JSONEncoder):
    def default(self, obj):
        if isinstance(obj, Decimal):
            return float(obj)
        return super(DecimalEncoder, self).default(obj)

def lambda_handler(event, context):
    operation = event.get('operation')
    
    if operation == 'CREATE':
        return create_student(event['student'])
    elif operation == 'READ':
        return read_student(event['studentId'])
    elif operation == 'UPDATE':
        return update_student(event['student'])
    elif operation == 'DELETE':
        return delete_student(event['studentId'])
    else:
        return {
            'statusCode': 400,
            'body': json.dumps('Unsupported operation')
        }
    
    # Create a student record
def create_student(student):
    try:
        table.put_item(Item=student)
        return {
            'statusCode': 200,
            'body': json.dumps('Student added successfully')
        }
    except ClientError as e:
        return {
            'statusCode': 500,
            'body': json.dumps(f'Error adding student: {e}')
        }
    
    # Read operation 
def read_student(studentId):
    try:
        # Make sure studentId is a string
        studentId = str(studentId)  # Ensuring the key is treated as a string
        # Fetch the student from DynamoDB
        response = table.get_item(Key={'studentId': studentId})
        
        # Check if item exists
        if 'Item' in response:
            # Serialize response to JSON
            return {
                'statusCode': 200,
                'body': json.dumps(response['Item'], cls=DecimalEncoder)
            }
        else:
            return {
                'statusCode': 404,
                'body': json.dumps(f"Student with studentId {studentId} not found")
            }
    except Exception as e:
        return {
            'statusCode': 500,
            'body': json.dumps(f"Error reading student: {str(e)}")
        }

# Update a student record
def update_student(student):
    try:
        response = table.update_item(
            Key={'studentId': student['studentId']},
            UpdateExpression="set #name = :name, age = :age, grade = :grade",
            ExpressionAttributeNames={'#name': 'name'},
            ExpressionAttributeValues={
                ':name': student['name'],
                ':age': student['age'],
                ':grade': student['grade']
            },
            ReturnValues="UPDATED_NEW"
        )
        return {
            'statusCode': 200,
            'body': json.dumps('Student updated successfully')
        }
    except ClientError as e:
        return {
            'statusCode': 500,
            'body': json.dumps(f'Error updating student: {e}')
        }

# Delete a student record
def delete_student(student_id):
    try:
        table.delete_item(Key={'studentId': student_id})
        return {
            'statusCode': 200,
            'body': json.dumps('Student deleted successfully')
        }
    except ClientError as e:
        return {
            'statusCode': 500,
            'body': json.dumps(f'Error deleting student: {e}')
        }
    