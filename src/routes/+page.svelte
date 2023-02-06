<script lang="ts" context="module">
  import { validateSchema } from "@felte/validator-zod";
  import { z } from "zod";
  import { createForm } from "felte";
</script>

<script lang="ts">
  const schema = z.object({
    email: z.string().email(),
    'traits.email': z.string().email(),
    password: z.string()
  });

  const { form, errors } = createForm({
    onSubmit: (values) => {
      // ...
    },
    validate: validateSchema(schema)
  });
</script>

{console.log(JSON.stringify($errors))}
<form use:form>
  <!-- This works as expected -->
  <input type="text" name="email" />
  {($errors['email'] ?? []).join("")}

  <!-- When the name of the field contains dot, This does not work. The errors object was incorrectly broken down -->
  <!-- {"email":null,"traits":{"email":null},"password":null} -->
  <input type="text" name="traits.email" />
  {($errors['traits.email'] ?? []).join("")}

  <input type="password" name="password" />
  <button type="submit">Sign In</button>
</form>
