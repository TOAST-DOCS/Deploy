## Dev Tools > Deploy > Error Code

## Ver 1.0

| resultCode | resultMessage |
| --------- | --------- |
| SUCCESS | success|
| BAD_REQUEST | The request is malformed or incorrect. |
| NOT_FOUND_ARTIFACT_INFO | The artifact info could not be found |
| ALREADY_UPLOADED_VERSION | The requested binary version already exists |
| NOT_FOUND_OS | The OS could not be found |
| NOT_FOUND_BINARY_GROUP | The binary group could not be found |
| NOT_FOUND_BINARY_INFO | The binary info could not be found |
| NOT_FOUND_BINARY_INFO | The binary is already deleted |
| FILE_SIZE_CAPACITY_EXCEEDED | Max file size exceeded |
| INVALID_FILE_EXTENSION | The filename extension is not matched with os |
| GENERAL_EXCEPTION | An unspecified error has occurred |
| NOT_FOUND_SERVER_GROUP | The server group could not be found |
| NOT_FOUND_SCENARIO | The scenario could not be found |
| ERROR_NO_AUTHENTICATION_ID | X-TC-AUTHENTICATION-ID header not exist |
| ERROR_NO_AUTHENTICATION_SECRET | X-TC-AUTHENTICATION-SECRET header not exist |
| ERROR_FAILED_UUID_API | Failed to User-Access-Key-Authentication |
| PERMISSION_DENIED | Permission denied |
| ERROR_INVALID_APP_KEY | Invalid AppKey |
| FAIL | fail |

## Previous version

| isSuccess | resultKey |
| --------- | --------- |
| true | - |
| false | ALREADY_UPLOADED_VERSION |
| false | INVALID_INFORMATION |
| false | NOT_FOUND_OS |
| false | ARTIFACT_ID_IS_NOT_NUMBER |
| false | BINARY_UPLOAD_ERROR |
| false | BINARY_GROUP_NOT_FOUND |
| false | BINARY_GROUP_IS_NOT_NUMBER |
| false | NOT_FOUND_BINARY_INFO |
| false | ALREADY_DELETED_BINARY |
| false | NOT_FOUND_BINARY |
| false | ALERT_ERROR |
| false | NOT_FOUND_AUTHORIZED_MEMBER |
| false | METAFILE_MODIFY_FAILED |
| false | EXCEED_FILE_CAPACITY |
