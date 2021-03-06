
.. code-block:: bash

    curl -s --user 'api:YOUR_API_KEY' \
	https://api.mailgun.net/v3/YOUR_DOMAIN_NAME/bounces \
	-F address='bob@example.com'

.. code-block:: java

 import javax.ws.rs.client.Client;
 import javax.ws.rs.client.ClientBuilder;
 import javax.ws.rs.client.Entity;
 import javax.ws.rs.client.WebTarget;

 import javax.ws.rs.core.Form;
 import javax.ws.rs.core.MediaType;

 import org.glassfish.jersey.client.authentication.HttpAuthenticationFeature;

 public class MGSample {

     // ...

     public static ClientResponse AddBounce() {

         Client client = ClientBuilder.newClient();
         client.register(HttpAuthenticationFeature.basic(
             "api",
             "YOUR_API_KEY"
         ));

         WebTarget mgRoot = client.target("https://api.mailgun.net/v3");

         Form reqData = new Form();
         reqData.param("address", "bob@example.com");

         return mgRoot
             .path("/{domain}/bounces")
             .resolveTemplate("domain", "YOUR_DOMAIN_NAME")
             .request(MediaType.APPLICATION_FORM_URLENCODED)
             .buildPost(Entity.entity(reqData, MediaType.APPLICATION_FORM_URLENCODED))
             .invoke(ClientResponse.class);
     }
 }

.. code-block:: php

  # Include the Autoloader (see "Libraries" for install instructions)
  require 'vendor/autoload.php';
  use Mailgun\Mailgun;

  # Instantiate the client.
  $mgClient = new Mailgun('YOUR_API_KEY');
  $domain = 'YOUR_DOMAIN_NAME';

  # Issue the call to the client.
  $result = $mgClient->post("$domain/bounces", array('address' => 'bob@example.com'));

.. code-block:: py

 def add_bounce():
     return requests.post(
         "https://api.mailgun.net/v3/YOUR_DOMAIN_NAME/bounces",
         auth=("api", "YOUR_API_KEY"),
         data={'address':'bob@example.com'})

.. code-block:: rb

 def add_bounce
   RestClient.post("https://api:YOUR_API_KEY"\
                   "@api.mailgun.net/v3/YOUR_DOMAIN_NAME/bounces",
                   :address => 'bob@example.com')
 end

.. code-block:: csharp

 using System;
 using System.IO;
 using RestSharp;
 using RestSharp.Authenticators;
 
 public class AddBounceChunk
 {
 
     public static void Main (string[] args)
     {
         Console.WriteLine (AddBounce ().Content.ToString ());
     }
 
     public static IRestResponse AddBounce ()
     {
         RestClient client = new RestClient ();
         client.BaseUrl = new Uri ("https://api.mailgun.net/v3");
         client.Authenticator =
             new HttpBasicAuthenticator ("api",
                                         "YOUR_API_KEY");
         RestRequest request = new RestRequest ();
         request.Resource = "{domain}/bounces";
         request.AddParameter ("domain", "YOUR_DOMAIN_NAME", ParameterType.UrlSegment);
         request.AddParameter ("address", "bob@example.com");
         request.Method = Method.POST;
         return client.Execute (request);
     }
 
 }

.. code-block:: go

 func AddBounce(domain, apiKey) error {
   mg := mailgun.NewMailgun(domain, apiKey, "")
   return mg.AddBounce("bob@example.com", "550", "Undeliverable message error")
 }
