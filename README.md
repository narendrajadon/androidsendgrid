# androidsendgrid
Sample Android app to send email using Sendgrid API.

Make sure to change username and password values in SendGridCredentials class.

Create SendGridAsyncTask class:-

public class SendGridAsyncTask extends AsyncTask<Hashtable<String, String>, Void, String> {
    @Override
    protected String doInBackground(Hashtable<String, String>... hashtables) {

        Hashtable<String, String> h = hashtables[0];
        SendGridCredentials sendGridCredentials = new SendGridCredentials();
        SendGrid sendGrid = new SendGrid(sendGridCredentials.getUsername(), sendGridCredentials.getPassword());
        sendGrid.addTo(h.get("to"));
        sendGrid.setFrom(h.get("from"));
        sendGrid.setSubject(h.get("subject"));
        sendGrid.setText(h.get("text"));
        String response = sendGrid.send();

        return response;
    }
}

Create SendGridCredentials class

Create method to send email from Activity:-

public class SendGridCredentials {
    public static final String username = "REPLACE WITH YOUR SENDGRID USERNAME";
    public static final String password = "REPLACE WITH YOUR SENDGRID PASSWORD";

    public String getUsername() {
        return username;
    }

    public String getPassword() {
        return password;
    }
}

private void sendMailUsingSendGrid(String from, String to, String subject, String mailBody){
        Hashtable<String, String> params = new Hashtable<>();
        params.put("to", to);
        params.put("from", from);
        params.put("subject", subject);
        params.put("text", mailBody);

        SendGridAsyncTask email = new SendGridAsyncTask();
        try{
            email.execute(params);
        }catch (Exception e){
            e.printStackTrace();
        }
    }
    
Call method on some event inside handler:-

              //On some event
                Handler handler = new Handler(Looper.getMainLooper());
                handler.post(new Runnable() {
                    @Override
                    public void run() {
                        sendMailUsingSendGrid(fromMail.getText().toString(), toMail.getText().toString(),
                                "This is the test subject",
                                "This is mail body send using sendgrid :)");

                        Toast.makeText(getApplicationContext(),"Sending mail...", Toast.LENGTH_SHORT).show();
                    }
                });
                
  That's it. :)




