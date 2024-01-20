# APIの役割の時のコントローラー
[scaffold_controller](https://github.com/Tarara33/TIL/blob/main/Rails/Rails%20g%20.md#scaffold_controller)を使用した場合の6アクション。    
(⚠️ RESTfulな APIでは、一般的には editアクションが不要で、代わりに updateアクションを使用してリソースを更新する。)
~~~
class TodosController < ApplicationController
  before_action :set_todo, only: %i[ show update destroy ]

  # GET /todos
  def index
    @todos = Todo.all

    ⭕️render json: @todos
  end

  # GET /todos/1
  def show
    ⭕️render json: @todo
  end

  # POST /todos
  def create
    @todo = Todo.new(todo_params)

    if @todo.save
      ⭕️render json: @todo, 💛status: :created, 🧡location: @todo
    else
      ⭕️render json: @todo.errors, 💛status: :unprocessable_entity
    end
  end

  # PATCH/PUT /todos/1
  def update
    if @todo.update(todo_params)
      ⭕️render json: @todo ❓
    else
      ⭕️render json: @todo.errors, status: :unprocessable_entity
    end
  end

  # DELETE /todos/1
  def destroy
    @todo.destroy
  end

  private
    # Use callbacks to share common setup or constraints between actions.
    def set_todo
      @todo = Todo.find(params[:id])
    end

    # Only allow a list of trusted parameters through.
    def todo_params
      params.require(:todo).permit(:title, :description, :completed)
    end
end
~~~
⭕️ JSON形式で返すようにしている。
***

### 💛 status
HTTPレスポンスのステータスコードの設定を意味する。

デフォルトでは`status: :ok`となり、200 OKとなる。  
新規作成時は、
新規作成時は`:created ( 201 Created)`にして、「リソースの作成が成功したこと」を示し、  
失敗時は`:unprocessable_entity (422 Unprocessable Entit)`にして、「リソースの作成ができなかったこと」を示す。
***

### 🧡 location
このオプションは、新しく作成されたリソースの URI（Uniform Resource Identifier）を指定する。  
locationヘッダーにこの URIが含まれ、「新しく作成されたリソースの URIがここにある」という情報をクライアントに提供する。
