title: 发送邮件
date: 2015-09-14 10:34:38
categories:
tags: [c#]
---

```
/// <summary>
/// 发送邮件
/// </summary>
/// <param name="request">发送邮件请求</param>
/// <returns></returns>
public bool SendEmail(string recipientEmailAddresses, string ccEmailAddresses, string subject, string body)
{
    #region 变量定义

    MailMessage email = new System.Net.Mail.MailMessage();
    System.Net.Mail.SmtpClient client = new System.Net.Mail.SmtpClient();
    Encoding encoding = System.Text.Encoding.UTF8;
    bool isBodyHtml = true;
    MailPriority mailPriority = MailPriority.High;

    #endregion

    #region 主体

    #region 组合收件人Email地址

    email.To.Add(recipientEmailAddresses);

    #endregion

    #region 组合抄送人Email地址

    if (!string.IsNullOrEmpty(ccEmailAddresses))
    {
        email.CC.Add(ccEmailAddresses);
    }

    #endregion

    email.From = new MailAddress(HJEmailAddress, HJEmailDisplayName, encoding);
    email.Subject = subject;
    email.SubjectEncoding = encoding;
    email.Body = body;
    email.BodyEncoding = encoding;
    email.IsBodyHtml = isBodyHtml;
    email.Priority = mailPriority;

    client.Credentials = new System.Net.NetworkCredential(HJEmailAddress, HJEmailPassword);
    client.Host = HJSmtpHost;
    client.Port = Convert.ToInt32(HJSmtpPort);
    client.EnableSsl = HJSmtpIsEnableSSL == "1" ? true : false;
    client.Timeout = 1000 * 60 * 3;

    try
    {
        client.Send(email);
    }
    catch (Exception ex)
    {
        return false;
    }

    #endregion

    #region 返回结果

    return true;

    #endregion
}
```