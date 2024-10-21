package com.task.service.impl;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.mail.SimpleMailMessage;
import org.springframework.mail.javamail.JavaMailSender;
import org.springframework.mail.javamail.MimeMessageHelper;
import org.springframework.scheduling.annotation.Async;
import org.springframework.stereotype.Service;

import jakarta.mail.MessagingException;
import jakarta.mail.internet.MimeMessage;

@Service
public class EmailService {

    @Autowired
    private JavaMailSender javaMailSender;

    @Async
    public void sendEmailOnRegister(String firstname, String lastname, String email) {
        String subject = "Welcome to TaskFlow! Simplify Your Tasks with Us";

        String content = "<html>" +
                "<head>" +
                "<style>" +
                "body { font-family: Arial, sans-serif; margin: 0; padding: 0; background-color: #f4f4f4; }" +
                ".container { width: 100%; max-width: 600px; margin: 0 auto; background: #ffffff; padding: 20px; border-radius: 8px; box-shadow: 0 0 10px rgba(0, 0, 0, 0.1); }" +
                "h1 { color: #333; }" +
                "p { color: #555; line-height: 1.6; }" +
                ".button { display: inline-block; padding: 10px 15px; margin-top: 20px; background-color: #007BFF; color: #ffffff; text-decoration: none; border-radius: 5px; }" +
                ".footer { margin-top: 20px; font-size: 12px; color: #aaa; text-align: center; }" +
                "</style>" +
                "</head>" +
                "<body>" +
                "<div class='container'>" +
                "<h1>Welcome to TaskFlow!</h1>" +
                "<p>Dear " + firstname+" "+lastname+ ",</p>" +
                "<p>Thank you for joining TaskFlow, your ultimate task management solution! We're excited to help you streamline your tasks and boost your productivity.</p>" +
                "<p>With TaskFlow, you can easily:</p>" +
                "<ul>" +
                "<li>Create and manage tasks effortlessly</li>" +
                "<li>Set deadlines and reminders</li>" +
                "<li>Collaborate with team members</li>" +
                "<li>Track your progress with ease</li>" +
                "</ul>" +
                "<p>To get started, simply log in to your account and explore all the features that TaskFlow has to offer!</p>" +
                "<a href='[Your Login URL]' class='button'>Get Started</a>" +
                "<div class='footer'>" +
                "Best Regards,<br>Your TaskFlow Team<br>" +
                "<a href='mailto:support@taskflow.com'>support@taskflow.com</a>" +
                "</div>" +
                "</div>" +
                "</body>" +
                "</html>";

        try {
            MimeMessage message = javaMailSender.createMimeMessage();
            MimeMessageHelper helper = new MimeMessageHelper(message, true);
            helper.setTo(email);
            helper.setSubject(subject);
            helper.setText(content, true); // Set true for HTML content

            javaMailSender.send(message);
        } catch (MessagingException e) {
            e.printStackTrace();
        }
    }
}
