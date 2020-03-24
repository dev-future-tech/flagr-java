# Flagr Java

An easy to use client for the [Flagr](https://checkr.github.io/flagr) feature flag service.

## General Concepts used in this documentation
 * **Flag** can be a feature flag or an experiment.
 * **Context** is the information that needs to be provided to evaluate a **flag**.
 * **Variant** represents the possible variation of a flag.
 * **Variant Attachment**  is additional meta information that you can return with your **variant**.

For more in depth information visit [Flagr official docs](https://checkr.github.io/flagr/#/home).

## Usage

Assume there's a flag called `color` for changing the blue background color of a page for a red one for 10% of the clients. Here is what the code for evaluating this flag would look like:

````java
flagr = new Flagr(FLAGR_HOST);
try {
    EvaluationResponse response = flagr.evaluate(
        new EvaluationContext("background_color")
    );
    System.out.println(response.getVariantKey()); // The variant will be red for 10% of evaluations
} catch (FlagrException e) {
    e.printStackTrace();
}
````

If one of the clients is testing the red background and only this client is supposed to see the red background?
The code above can send the client properties to Flagr so it knows about the client. Then on Flagr Admin UI you can specify that only your test client will see the red background.
The code for this example would look like:
````java
flagr = new Flagr(FLAGR_HOST);
try {
    EvaluationContext context = new EvaluationContext("background_color");
    context.setEntityContext(client); // Sends client info to Flagr so you can filter by one of it's properties on the UI.
    EvaluationResponse response = flagr.evaluate(context);
    System.out.println(response.getVariantKey()); // This would return red for the client(s) you select on Flagr UI.
} catch (FlagrException e) {
    e.printStackTrace();
}
````

If the variant needs properties as well, this can be achieved by creating
a variant attachment and describing it's properties on Flagr UI. Suppose
color has the properties name and hexadecimal, the code would look like:
````java
flagr = new Flagr(FLAGR_HOST);
try {
    EvaluationContext context = new EvaluationContext("background_color");
    context.setEntityContext(client); // Sends client info to Flagr so you can filter by one of it's properties on the UI.
    EvaluationResponse response = flagr.evaluate(context);
    Color color = response.getVariantAttachment(Color.class);
    System.out.println(response.getVariantKey(color.getName())); // This would return red for the client(s) you select on Flagr UI.
    System.out.println(response.getVariantKey(color.getHex())); // This would return #FF0000 for example.
} catch (FlagrException e) {
    e.printStackTrace();
}
````

## Versioning

We use [SemVer](http://semver.org/) for versioning.