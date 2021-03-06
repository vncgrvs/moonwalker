<template>
  <div
    class="
      w-full
      flex-row
      justify-center
      h-full
      min-h-screen
      py-12
      px-6
      bg-gray-300 bg-opacity-50
      font-lix
      overflow-hidden
    "
  >
    <!-- Step Overview -->
    <div data-theme="vg" class="flex w-full h-full p-5">
      <ul class="steps w-full">
        <li
          class="step"
          v-if="
            !this.$store.state.firstStepError && !this.$store.state.firstStep
          "
        >
          Create Workspace
        </li>
        <li
          v-if="this.$store.state.firstStepError"
          :class="{ 'step-error': this.$store.state.firstStepError }"
          class="step"
          data-content="✕"
        >
          Create Workspace
        </li>
        <li
          :class="stepOne"
          class="step"
          v-if="this.$store.state.firstStep"
          data-content="✓"
        >
          Created Workspace
        </li>

        <li
          :class="stepTwo"
          class="step"
          v-if="
            !this.$store.state.secondStepError && !this.$store.state.secondStep
          "
        >
          Provision Data Model
        </li>
        <li
          v-if="this.$store.state.secondStepError"
          :class="{ 'step-error': this.$store.state.secondStepError }"
          class="step"
          data-content="✕"
        >
          Provision Data Model
        </li>
        <li
          :class="stepTwo"
          class="step"
          v-if="this.$store.state.secondStep"
          data-content="✓"
        >
          Provisioned Data Model
        </li>
        <li
          :class="stepThree"
          class="step"
          v-if="
            !this.$store.state.thirdStepError && !this.$store.state.thirdStep
          "
        >
          Upload Processors
        </li>
        <li
          v-if="this.$store.state.thirdStepError"
          :class="{ 'step-error': this.$store.state.thirdStepError }"
          class="step"
          data-content="✕"
        >
          Upload Processors
        </li>

        <li
          :class="stepThree"
          class="step"
          v-if="this.$store.state.thirdStep"
          data-content="✓"
        >
          Uploaded Processors 🎉
        </li>
      </ul>
    </div>

    <!-- Input Form -->
    <div class="w-full h-full p-5">
      <form
        class="flex flex-col items-center"
        v-on:submit.prevent=""
        v-on:submit="sendConfig"
      >
        <div class="w-1/2">
          <!-- Instance Selector -->
          <div class="mt-1 relative rounded-md shadow-sm">
            <label
              for="instance"
              class="block text-sm font-medium text-gray-700"
              >Choose an instance:</label
            >

            <select
              name="instance"
              required
              autofocus
              :disabled="this.$store.state.firstStep"
              id="instance"
              v-model="instance"
              class="
                focus:ring-indigo-500
                focus:border-indigo-500
                block
                pl-7
                pr-12
                sm:text-sm
                border-gray-300
                rounded-md
                p-2
                w-full
              "
            >
              <option v-for="key in this.$store.state.hostOptions" :key="key">
                {{ key }}
              </option>
            </select>
          </div>

          <!-- Workspace Name Selector -->
          <div class="mt-6 relative rounded-md shadow-sm">
            <label
              for="workspaceName"
              class="block text-sm font-medium text-gray-700"
              >Choose a workspace name:</label
            >

            <input
              name="workspaceName"
              v-model="workspaceName"
              id="workspaceName"
              :class="{
                'bg-gray-100': !this.$store.state.showCreateWS,
                'text-gray-600': !this.$store.state.showCreateWS,
              }"
              required
              
              :disabled="this.$store.state.firstStep"
              class="
                focus:ring-indigo-500
                focus:border-indigo-500
                block
                pl-7
                pr-12
                sm:text-sm
                border-gray-300
                rounded-md
                p-2
                w-full
              "
            />
          </div>
        </div>

        <!-- API Token Select -->
        <div class="w-6/12" v-if="this.$store.state.showCreateWS">
          <label
            for="apiToken"
            class="block text-sm font-medium text-gray-700 mt-6"
            >Enter Superadmin API Token:</label
          >

          <!-- Tooltip -->
          <div class="mt-1 w-full rounded-md shadow-sm inline-block">
            <input
              type="password"
              name="apiToken"
              id="apiToken"
              required
              v-model="apiToken"
              class="
                focus:ring-indigo-500
                focus:border-indigo-500
                block
                pl-7
                pr-12
                sm:text-sm
                border-gray-300
                rounded-md
                p-2
                w-full
              "
              placeholder=""
            />
          </div>
          <div class="absolute inline-block align-middle">
            <button
              v-if="!this.$store.state.superAdminToolTip"
              type="button"
              class="relative p-2 pt-3 inline-block "
              @click="openTooltip"
            >
              <svg
                xmlns="http://www.w3.org/2000/svg"
                class="h-6 w-6 stroke-current text-lix hover:opacity-60"
                fill="none"
                viewBox="0 0 24 24"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"
                />
              </svg>
            </button>
            <!-- Tooltip Popup -->
            <div
              v-if="this.$store.state.superAdminToolTip"
              class="
                fixed
                top-1/4
                
                left-1/4
                flex flex-col
                items-center
                z-50
                w-8/12
                h-auto
                rounded-md
                shadow-md
                bg-white
                text-gray-600 text-xs
                font-light
                p-3
              "
            >
              <div>
                <p class="font-medium text-lg">
                  How to create a super admin token?
                </p>

                <div class=" flex flex-row overflow-hidden mt-4 ">
                  <div class="w-1/2 ">
                    <p class="text-center font-bold mb-2">Watch How-To Video</p>
                    <a
                      href="https://leanix.atlassian.net/wiki/spaces/CNS/pages/1341915187/CNS%20Customer%20Success%20Enablement"
                      target="_blank" 
                      rel="noopener noreferrer"
                    >
                      <img class="object-cover " src="~/assets/img/thumbnail-tooltip.png" />
                    </a>
                  </div>

                  <div class="divider divider-vertical justify-bottom"></div>

                  <div class="w-1/2 items-center ">
                  <p class="text-center font-bold mb-2">Read Confluence Step-by-Step Guide</p>
                  <a
                      href="https://leanix.atlassian.net/wiki/spaces/CNS/pages/1341915187/CNS%20Customer%20Success%20Enablement"
                      target="_blank" 
                      rel="noopener noreferrer"
                      
                    >
                      <img class="object-cover h-full" src="~/assets/img/thumbnail-confluence.png" />
                    </a>
                    </div>
                </div>
              </div>

              <button
                type="button"
                class="
                  bg-green-400
                  rounded-md
                  text-sm text-white
                  p-2
                  mt-6
                  w-1/4
                  hover:opacity-90
                "
                @click="openTooltip"
              >
                close
              </button>
            </div>
          </div>
        </div>

        <!-- Workspace Edition Selector -->
        <div class="w-1/2" v-if="!this.$store.state.showCreateWS">
          <div class="mt-6 relative rounded-md shadow-sm">
            <label
              for="wsEdition"
              class="block text-sm font-medium text-gray-700"
              >Choose an workspace edition:</label
            >

            <select
              name="wsEdition"
              v-model="edition"
              id="wsEdition"
              required
              class="
                focus:ring-indigo-500
                focus:border-indigo-500
                block
                pl-7
                pr-12
                sm:text-sm
                border-gray-300
                rounded-md
                p-2
                w-full
              "
            >
              <option v-for="key in this.$store.state.wsOptions" :key="key">
                {{ key }}
              </option>
            </select>
          </div>
        </div>

        <!-- Error Message -->
        <div
          v-if="this.$store.state.apiError"
          class="flex flex-col mt-6 w-1/2 bg-red-600 rounded-md h-auto p-4"
        >
          <div class="mb-2">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="h-6 w-6 stroke-current text-white inline-block mr-2"
              fill="none"
              viewBox="0 0 24 24"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M9.172 16.172a4 4 0 015.656 0M9 10h.01M15 10h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"
              />
            </svg>
            <p class="font-bold text-white inline-block">Ooops!</p>
          </div>
          <div
            class="flex flex-wrap w-full bg-gray-100 rounded-md opacity-80 p-3"
          >
            <pre
              class="
                text-black text-xs
                font-light
                w-full
                break-words
                whitespace-pre-wrap
              "
            >
              {{ this.$store.state.apiErrorMessage }}
            </pre>
          </div>
        </div>

        <!-- Success Message -->

        <div
          v-if="this.$store.state.thirdStep"
          class="
            flex
            mt-6
            w-1/2
            bg-green-400
            rounded-md
            h-auto
            p-4
            items-center
          "
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            class="h-10 w-10 stroke-current text-white"
            fill="none"
            viewBox="0 0 24 24"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z"
            />
          </svg>

          <div
            class="text-white text-lg flex flex-col ml-6 w-auto font-semibold"
          >
            <div class="">
              <p class="inline-block font-light text-lg">Created workspace:</p>
              <p class="inline-block">
                {{ this.$store.state.workspace.workspaceName }}
              </p>
            </div>

            <div>
              <p class="inline-block font-light">Workspace ID:</p>
              <p class="inline-block">
                {{ this.$store.state.installParams.workspaceId }}
              </p>
            </div>

            <div>
              <p class="inline-block font-light">URL:</p>
              <a
                class="inline-block"
                v-bind:href="this.$store.state.workspace.workspaceUrl"
                >{{ this.$store.state.workspace.workspaceUrl }}</a
              >
            </div>
          </div>
        </div>

        <!-- Create Workspace Button -->
        <button
          data-theme="vg"
          v-if="this.$store.state.showCreateWS && !this.$store.state.apiError"
          :class="{ loading: this.$store.state.apiBusy }"
          class="mt-6 btn btn-primary rounded-md py-2 w-1/4 btn"
          type="submit"
        >
          Create Workspace
        </button>

        <!-- Install Workspace -->
        <button
          data-theme="vg"
          v-if="
            !this.$store.state.showCreateWS &&
            !this.$store.state.thirdStep &&
            !this.$store.state.apiError
          "
          class="btn btn-primary mt-6 rounded-md py-2 w-1/4"
          :class="{ loading: this.$store.state.apiBusy }"
          type="submit"
        >
          Install Edition
        </button>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  name: "settings",
  data() {
    return {};
  },
  methods: {
    openTooltip() {
      this.$store.commit("setTooltipSuperAdmin");
    },
    goToWorkspace() {
      let workspaceUrl = this.$store.state.workspace.workspaceUrl;
      console.log(workspaceUrl);
      window.location = workspaceUrl;
    },
    sendConfig() {
      let apiBusy = this.$store.state.apiBusy;

      // dont allow for click bashing
      if (!apiBusy) {
        if (this.$store.state.firstStep) {
          // if workspace has already been created

          this.$store.dispatch("installWorkspace");
        } else {
          // if workspace has not already been created
          this.$store.dispatch("createWorkspace");
        }
      }
    },
  },
  computed: {
    stepOne: function () {
      return {
        "step-primary": this.$store.state.firstStep,
        "step-success": this.$store.state.firstStep,
      };
    },
    stepTwo: function () {
      return {
        "step-primary": this.$store.state.secondStep,
        "step-success": this.$store.state.secondStep,
      };
    },
    stepThree: function () {
      return {
        "step-primary": this.$store.state.thirdStep,
        "step-success": this.$store.state.thirdStep,
      };
    },

    instance: {
      get() {
        return this.$store.state.workspace.instance;
      },
      set(value) {
        this.$store.commit("updateInstance", value);
      },
    },

    workspaceName: {
      get() {
        return this.$store.state.workspace.workspaceName;
      },
      set(value) {
        this.$store.commit("updateWorkspaceName", value);
      },
    },

    edition: {
      get() {
        return this.$store.state.installParams.edition;
      },
      set(value) {
        this.$store.commit("updateWorkspaceEdition", value);
      },
    },

    apiToken: {
      get() {
        return this.$store.state.workspace.apiToken;
      },
      set(value) {
        this.$store.commit("updateApiToken", value);
      },
    },
  },
};
</script>

<style>
</style>