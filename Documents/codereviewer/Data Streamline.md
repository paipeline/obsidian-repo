1. webhooks listener:handle_gitlab_webhook -> this classifies the action taken by the users
2. webhook_controller:handle_merge_event -> Process the metadata.
3. WEebhookService:handle_event -> processes the metadata 
	1. CommentService-> detects note_body