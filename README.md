# api-refetch

`api-refetch` is a React hook library designed to simplify the management of asynchronous API requests within React applications. This library provides a powerful and flexible hook, `useAsync`, that handles fetching, caching, retries, and state management of asynchronous operations.

## Features

- **Easy to use**: Integrate asynchronous data fetching into your components with minimal boilerplate.
- **State management**: Automatically manages loading, error, success, and fetched states.
- **Retry Logic**: Configurable retry mechanism for failed requests.
- **Caching**: Optional caching of requests to improve performance and reduce server load.
- **Revalidation**: Supports automatic revalidation of data to keep the UI up to date.
- **Customizable**: Extensive options to tailor the behavior of the hook to your needs.

## Installation

Install the package via npm:

```bash
npm install api-refetch
```

or using yarn:

```bash
yarn add api-refetch
```

## Usage

Here's a simple example of how to use `api-refetch` to fetch user data from an API:

### Setting up the Async Function

First, define the asynchronous function that fetches your data. For example, fetching user data:

```javascript
const fetchUserData = async (userId) => {
  const response = await fetch(`/api/users/${userId}`);
  if (!response.ok) {
    throw new Error("Failed to fetch");
  }
  return await response.json();
};
```

### Using the `useAsync` Hook

Now, use the `useAsync` hook in your component:

```javascript
import { useAsync } from "api-refetch";

const UserDetails = ({ userId }) => {
  const { isLoading, data, error, revalidate } = useAsync(
    "userDetails",
    () => fetchUserData(userId),
    {
      cache: true,
      revalidateTime: 60000, // 1 minute
      autoFetch: true,
      onSuccess: (data) => console.log("User data fetched successfully:", data),
      onError: (error) => console.error("Error fetching user data:", error),
    }
  );

  if (isLoading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;
  return (
    <div>
      <h1>{data.name}</h1>
      <button onClick={() => revalidate()}>Refresh</button>
    </div>
  );
};
```

## Contributing

Contributions are welcome! Please open an issue or submit a pull request with your suggestions or improvements.

## License

`api-refetch` is open-source software licensed under the MIT License. See the [LICENSE](LICENSE) file for more details.
