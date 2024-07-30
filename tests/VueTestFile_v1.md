---
  title: 'VueTestFile.vue'
  description: 'component description'
  pubDate: 'July 30, 2024'
  author: 'Carlos Polanco'
  ---
  
  
  
  # VueTestFile.vue
  ## Explanation of the Code

This code snippet is a Vue.js component that allows users to drag and drop files or click to select files. Let's break down the key parts of the code:

### Template Section
- The template section defines the HTML structure of the component.
- It includes a `div` element that serves as a drop zone for files, an `input` element for file selection (hidden), and a label for styling and interaction.
- The `@dragover.prevent` and `@drop.prevent` are event listeners that call the `handleDragOver` and `handleDrop` methods respectively.
- The component uses Tailwind CSS classes for styling.

### Script Section
- The script section contains the Vue.js component logic.
- It imports two icons (`DocumentTextIcon` and `ArrowUpTrayIcon`) from the `@heroicons/vue` library.
- The component's data includes `allowedExtensions` (an array of allowed file extensions), `selectedFiles` (an array to store selected files), and `maxFiles` (maximum number of files allowed).
- The component defines methods like `handleDragOver`, `handleDrop`, `extractFiles`, `traverseFileTree`, `isValidFile`, `handleFileSelect`, and `validateAndSetFiles` to handle file operations.
- These methods are responsible for validating files, extracting files from drag and drop events, setting selected files, and emitting errors if needed.

### Style Section
- The style section contains component-specific styles using scoped CSS.

### Example Usage
To use this component in a Vue.js application, you would include this code in a `.vue` file and import any necessary dependencies. Here's an example of how you might use this component:

```vue
<template>
  <div>
    <FileUploader @setError="handleError" />
  </div>
</template>

<script>
import FileUploader from './FileUploader.vue';

export default {
  components: {
    FileUploader
  },
  methods: {
    handleError(error) {
      // Handle errors here
      console.error(error);
    }
  }
};
</script>
```

In this example, the `FileUploader` component is used within a parent component, and an `setError` event is listened to handle any errors emitted by the `FileUploader` component.
  
  ## Component Code
  ```jsx
  <template>
    <div
      @dragover.prevent="handleDragOver"
      @drop.prevent="handleDrop"
      class="p-4 border-2 border-dashed border-gray-300 rounded-md text-center cursor-pointer mb-4 h-96 w-96 flex overflow-y-scroll items-center justify-center"
    >
      <input
        type="file"
        @change="handleFileSelect"
        style="display: none;"
        :accept="allowedExtensions.join(',')"
        id="fileUpload"
        multiple
      />
      <label for="fileUpload" class="cursor-pointer">
        <div v-if="selectedFiles.length">
          <div class="flex flex-wrap gap-10">
            <div v-for="file in selectedFiles" :key="file.name">
              <DocumentTextIcon class="mx-auto w-8" />
              <p>{{ file.name }}</p>
            </div>
          </div>
        </div>
        <div v-else>
          <ArrowUpTrayIcon class="mx-auto w-8" />
          <p>Drag & drop files or folders here, or click to select files</p>
        </div>
      </label>
    </div>
  </template>
  
  <script>
  import { DocumentTextIcon, ArrowUpTrayIcon } from "@heroicons/vue/24/outline";
  
  export default {
    components: {
      DocumentTextIcon,
      ArrowUpTrayIcon
    },
    data() {
      return {
        allowedExtensions: [
          ".js", ".jsx", ".ts", ".tsx", ".py", ".java", ".rb", ".php",
          ".html", ".css", ".cpp", ".c", ".go", ".rs", ".swift", ".kt",
          ".m", ".h", ".cs", ".json", ".xml", ".sh", ".yml", ".yaml",
          ".vue", ".svelte", ".qwik"
        ],
        selectedFiles: [],
        maxFiles: 4
      };
    },
    methods: {
      handleDragOver(event) {
        event.preventDefault();
      },
      handleDrop(event) {
        const items = event.dataTransfer.items;
        this.extractFiles(items).then(files => this.validateAndSetFiles(files));
      },
      async extractFiles(items) {
        const filesArray = [];
        for (let i = 0; i < items.length; i++) {
          const item = items[i].webkitGetAsEntry();
          if (item.isFile) {
            const file = await new Promise(resolve => item.file(resolve));
            if (this.isValidFile(file)) {
              filesArray.push(file);
            } else {
              this.$emit("setError", `Invalid file type. Only ${this.allowedExtensions.join(", ")} files are allowed.`);
            }
          } else if (item.isDirectory) {
            await this.traverseFileTree(item, filesArray);
          }
        }
        return filesArray;
      },
      traverseFileTree(item, filesArray) {
        return new Promise(resolve => {
          if (item.isFile) {
            item.file(file => {
              if (this.isValidFile(file)) {
                filesArray.push(file);
              }
              resolve();
            });
          } else if (item.isDirectory) {
            const dirReader = item.createReader();
            dirReader.readEntries(async entries => {
              for (const entry of entries) {
                await this.traverseFileTree(entry, filesArray);
              }
              resolve();
            });
          }
        });
      },
      isValidFile(file) {
        return this.allowedExtensions.some(ext => file.name.endsWith(ext));
      },
      handleFileSelect(event) {
        const files = Array.from(event.target.files);
        this.validateAndSetFiles(files);
      },
      validateAndSetFiles(files) {
        const filteredFiles = files.filter(file => this.isValidFile(file));
        if (filteredFiles.length > this.maxFiles) {
          this.$emit("setError", `You can only upload a maximum of ${this.maxFiles} files.`);
        } else {
          this.selectedFiles = filteredFiles.slice(0, this.maxFiles);
          this.$emit("setError", "");
        }
      }
    }
  };
  </script>
  
  <style scoped>
  /* Add any component-specific styles here */
  </style>
  ```
  