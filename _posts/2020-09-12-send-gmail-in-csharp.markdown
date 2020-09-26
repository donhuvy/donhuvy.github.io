---
layout: post
title:  "Send email in C#"
date:   2020-09-12 16:41:00 +0700
categories: csharp programming
---
Send email in C# by Gmail SMTP.

{% highlight csharp %}
public static void sendMailByGmail(string toEmail, string subject, string content)
{
    string fromAddress = "trustee2020app@gmail.com";
    var toAddress = new MailAddress(toEmail, toEmail);
    const string fromPassword = "a@A!@#$%^9999";
    using (MailMessage mail = new MailMessage())            {
        mail.From = new MailAddress(fromAddress);
        mail.To.Add(toAddress);
        mail.Subject = subject;
        mail.Body = content;
        mail.IsBodyHtml = true;
        //mail.Attachments.Add(new Attachment("C:\\file.zip"));
        using (SmtpClient smtp = new SmtpClient("smtp.gmail.com", 587))
        {
            smtp.UseDefaultCredentials = false;
            smtp.Credentials = new NetworkCredential(fromAddress, fromPassword);
            smtp.EnableSsl = true;
            smtp.Send(mail);
        }
    }
}
{% endhighlight %}

Here, have some $$\pi$$.

The greatest equation known to man is: 

$$e^{ix} = \cos{x} + i\sin{x}$$
