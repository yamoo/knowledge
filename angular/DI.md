https://angular.jp/guide/architecture-services

### ルートレベル

```
@Injectable({
 providedIn: 'root',
})
```

- ルートレベルでサービスを提供すると、Angularは**共有インスタンスを1つ作成し、 それを求めるクラスに注入します**。
- @Injectable() メタデータにプロバイダーを登録することは、サービスが使用されない場合にコンパイルされるアプリケーションからサービスを削除してアプリケーションを最適化することを、 Angularに許可することにもなります。

### プロバイダーレベル

```
@NgModule({
  providers: [
  BackendService,
  Logger
 ],
 ...
})
```

- プロバイダーを特定の NgModule に登録すると、**そのNgModule内のすべてのコンポーネントで同じサービスのインスタンスを使用できます**。このレベルで登録するには、@NgModule() デコレーターの providers プロパティを使用します。

### コンポーネントレベル

```
@Component({
  selector:    'app-hero-list',
  templateUrl: './hero-list.component.html',
  providers:  [ HeroService ]
})

コンポーネントレベルでプロバイダーを登録すると、**そのコンポーネントの新しいインスタンスごとに 新しいサービスインスタンスが取得されます**。
```
