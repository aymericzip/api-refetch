# api-refetch

`api-refetch` is a React hook library designed to simplify the management of asynchronous API requests within React applications. This library provides a powerful and flexible hook, `useAsync`, that handles fetching, caching, retries, and state management of asynchronous operations.

## Features

- **Easy to use**: Integrate asynchronous data fetching into your components with minimal boilerplate.
- **State management**: Automatically manages loading, error, success, and fetched states.
- **Storage**: Built-in support for storing and retrieving data in session storage.
- **Caching**: Optional caching of requests to improve performance and reduce server load.
- **Retry Logic**: Configurable retry mechanism for failed requests.
- **Revalidation**: Supports automatic revalidation of data to keep the UI up to date.
- **Parallel Component Mount Fetching**: api-refetch provides a mechanism to handle parallel component mount fetching, ensuring that only one fetch request is initiated at a time, regardless of the number of components mounting simultaneously.
- **Customizable**: Extensive options to tailor the behavior of the hook to your needs.

## Installation

Install the package via npm:

```bash
npm install api-refetch
```

using pnpm:

```bash
pnpm add api-refetch
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
  const { isLoading, data, error, revalidate, setData } = useAsync(
    "userDetails",
    () => fetchUserData(userId),
    {
      enable: true, // enable the hook
      cache: true, // cache the API call result using zustand
      store: true, // store the API call result in the session storage
      revalidateTime: 60 * 1000, // revalidate every 1 minute
      retryLimit: 3, // retry 3 times if the API call fails
      retryTime: 10 * 1000, // wait 10 seconds before retrying
      autoFetch: true, // auto fetch the API call when the component is mounted
      revalidation: true, // enable revalidation
      revalidateTime: 5 * 60 * 1000, // revalidate every 5 minutes
      isInvalidated: false, // determine if the data is invalidated and should be refetched
      invalidateQueries: ["user"], // invalidate other queries when the data is updated
      updateQueries: ["user"], // set other queries data when the data is updated
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
      <button onClick={() => setData(null)}>Clear</button>
    </div>
  );
};
```

### Using the `useAsyncWrapper` Hook

The `useAsyncWrapper` hook is a convenient way to define a custom hook that wraps an asynchronous function. It simplifies the process of declaring and using the `useAsync` hook in your components.

Here's an example of how to use the `useAsyncWrapper` hook to define a custom hook for fetching user data:

```javascript
import { useAsyncWrapper } from "api-refetch";

const fetchUserData = async (userId) => {
  const response = await fetch(`/api/users/${userId}`);
  if (!response.ok) {
    throw new Error("Failed to fetch");
  }
  return await response.json();
};

const useUserDetails = useAsyncWrapper("userDetails", fetchUserData, {
  // Additional options for the useAsync hook
});
```

Now, you can use the `useUserDetails` hook in your components:

```javascript
import { useAsync } from "api-refetch";

const UserDetails = ({ userId }) => {
  const { isLoading, data, error, revalidate, setData } =
    useAsyncWrapper(userId);

  if (isLoading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;
  return (
    <div>
      <h1>{data.name}</h1>
      <button onClick={() => revalidate()}>Refresh</button>
      <button onClick={() => setData(null)}>Clear</button>
    </div>
  );
};
```

## Why Choose `api-refetch` Over React Query or SWR?

While React Query and SWR are excellent tools for data fetching and caching in React applications, `api-refetch` offers unique advantages that may make it more suitable for certain use cases:

- **Zustand-based Storage**: `api-refetch` utilizes Zustand for state management, which avoids the need for a context provider or wrapper around your application, simplifying integration and reducing boilerplate.

- **Preset data using Session Storage**: `api-refetch` provides a built-in mechanism for storing and retrieving data in session storage, allowing you to easily cache API responses and reuse them across your application. It can be use to accelerate the loading of our data before automatic revalidation.

- **No Wrapper Store Provider Required**: Unlike other libraries that require you to wrap your application with a provider for state management, `api-refetch` operates without such requirements, making it easier to integrate with existing projects or for those seeking less complexity.

- **Lightweight**: With a size of only 91.6 kB, `api-refetch` is significantly lighter than React Query (2.26 MB) and SWR (620 kB), making it an excellent choice for projects where payload size is a critical factor.

- **Optimized for API Fetching**: This library is specifically optimized for API fetching scenarios within npm/react packages, providing just enough functionality to manage async operations effectively without over-engineering solutions.

- **Easily Customizable and Extendable**: The codebase of `api-refetch` is designed to be easily understandable and modifiable. Developers can quickly copy the code into their applications and modify it according to their specific needs, providing a high degree of flexibility.

- **Focused Functionality**: `api-refetch` focuses solely on the essentials of fetching, caching, and state management without bundling additional features that might not be necessary for all projects, thus keeping it streamlined and efficient.

By choosing `api-refetch`, developers can leverage a straightforward, highly customizable solution that integrates seamlessly with modern React applications, providing an efficient and effective alternative to more heavyweight options.

This section aims to clearly articulate the unique selling points of `api-refetch`, helping users to make an informed decision based on their specific requirements and the relative strengths of this library compared to other options in the market.

## Contributing

Contributions are welcome! Please open an issue or submit a pull request with your suggestions or improvements.

## License

`api-refetch` is open-source software licensed under the MIT License. See the [LICENSE](LICENSE) file for more details.
