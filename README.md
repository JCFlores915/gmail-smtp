# gmail-smtp
This is how I send email by using gmail to client with PHP script. 

In your PHP script, just call the php function send_mail($to,$cc,$bcc,$subject,$body,$attach).

In the PHP script, you cannot send all the email in one go. I coded the script, to refresh after 5 emails. 

That's mean, if you have a lot of email to send it will take some times. I have tried send to 700 emails from one gmail account. 


require("/phpmailer/class.phpmailer.php");

function send_mail($to,$cc,$bcc,$subject,$body,$attach)
{	
	$mail = new PHPMailer(true); 
	$mail->SMTPDebug = 0;
	$mail->IsSMTP(); // send via SMTP
	$mail->SMTPAuth = true; // turn on SMTP authentication
	$mail->Username = "ABC@gmail.com"; // SMTP username 
	$mail->Password = "PASSWORD"; // SMTP password
	$mail->FromName = "Webmaster"; // SMTP sender's name 
	
	
	if($to)
	{	
		if(is_array($to))
		{	
			for($i=0;$i<sizeof($to);$i++)
				$mail->AddAddress($to[$i],"");
		}
		else
			$mail->AddAddress($to,"");
	}
	if($cc)
	{
		if(is_array($cc))
		{	
			for($i=0;$i<sizeof($cc);$i++)
				$mail->AddCC($cc[$i],"");
		}
		else
			$mail->AddCC($cc,"");
	}
	
	
	if($bcc)
	{
		if(is_array($bcc))
		{	
			for($i=0;$i<sizeof($bcc);$i++)
				$mail->AddBCC($bcc[$i],"");
		}
		else
			$mail->AddBCC($bcc,"");
	}

	$mail->WordWrap = 50; // set word wrap
	$mail->IsHTML(true); // send as HTML
	$mail->Subject = $subject;
	$mail->Body = $body."<br><br>***This is a computer generated email, please do NOT reply to this email."; //HTML Body
	
	if($attach)
	{
		if(is_array($attach))
		{
			for($i=0;$i<sizeof($attach);$i++)
				$mail->AddAttachment($attach[$i],$attach[$i],'base64','application/octet-stream');   // AddAttachment($path, $name = "", $encoding = "base64", $type = "application/octet-stream")
		}
		else
			$mail->AddAttachment($attach,$attach,'base64','application/octet-stream');
	}
		
	//$mail->AddEmbeddedImage('address.pdf', 'PDFFile', 'PDF.zip');
	
	if(!$mail->Send()) 
	{
		echo "There was an error sending the message";
		exit;
	}
	echo "Message was sent successfully";
}



