<script>
  import { getClient, Body } from "@tauri-apps/api/http";
  let httpMethod = "GET";
  let httpUrl = "https://jsonplaceholder.typicode.com/todos/1";
  let httpBody = "";

  export let onMessage;

  async function makeHttpRequest() {
    const client = await getClient().catch(e => {
      onMessage(e)
      throw e
    });
    let method = httpMethod || "GET";
    let url = httpUrl || "";

    const options = {
      url: url || "",
      method: method || "GET",
    };

    if (
      (httpBody.startsWith("{") && httpBody.endsWith("}")) ||
      (httpBody.startsWith("[") && httpBody.endsWith("]"))
    ) {
      options.body = Body.json(JSON.parse(httpBody));
    } else if (httpBody !== "") {
      options.body = Body.text(httpBody);
    }

    client.request(options).then(onMessage).catch(onMessage);
  }
</script>

<form on:submit|preventDefault={makeHttpRequest}>
  <select class="button" id="request-method" bind:value={httpMethod}>
    <option value="GET">GET</option>
    <option value="POST">POST</option>
    <option value="PUT">PUT</option>
    <option value="PATCH">PATCH</option>
    <option value="DELETE">DELETE</option>
  </select>
  <input
    id="request-url"
    placeholder="Type the request URL..."
    bind:value={httpUrl}
  />
  <br />
  <textarea
    id="request-body"
    placeholder="Request body"
    rows="5"
    bind:value={httpBody}
  />
  <button class="button" id="make-request"> Make request </button>
</form>

<style>
#request-body {
  width:100%;
  margin-right:10px;
  font-size:12px;
}
</style>
